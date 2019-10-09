- [转UnsatisfiedLinkError常见问题及解决方案](http://www.cnblogs.com/daxingxing/p/6197234.html)
```
[持续更新]UnsatisfiedLinkError常见问题及解决方案
想必很多开发者和我们一样，遇到过许多UnsatisfiedLinkError的困难，着实令人头疼，现在总结一下，希望能帮助更多的人。

常见错误

lib库不同目录下的SO文件参差不齐。
lib库目录下的SO不符合相应的CPU架构。
64-bit下使用System.load加载SO："lib_xyz.so" is 32-bit instead of 64-bit
java代码混淆导致。
注册方式不对，或已经被其他类注册。
empty/missing DT_HASH in "libxxxx.so" (built with --hash-style=gnu?)
出错现象及解决方案

lib库不同目录下的SO文件参差不齐。
发现很多APK包打出来，lib目录下同时带着armeabi、armeabi-v7a，但是armeabi目录下可能有3个SO，而armeabi-v7a下只有2个SO，更有甚者还有armeabi、armeabi-v7a、x86、x86_64、arm64-v8a全部都有，但是不同目录下的SO个数都不一样。

这样打出来的APK包，在安装的时候会让Android系统“很为难”，它搞不清到底该选择哪个SO来安装。有时可能会造成某个SO的漏安装，那么在APP运行的时候加载SO时就会出现异常了。

解决方案：

1、只保留lib下的一个目录足够（armeabi或armeabi-v7a保留一个），其他目录全部不用配置。

2、如果想继续多配置几个CPU架构的lib目录，那就全部配置齐全。实际上有时候很难做到，特别是当需要使用三方库的SO的时候，往往并不那么容易找的齐全。由于全部打齐全会对APK的体积有增加，所以还是推荐第一种方案。

lib库目录下的SO不符合相应的CPU架构。

同上面的问题差不多，有些APK包打出来，同时配置了armeabi和arm64-v8，但是却在arm64-v8放置了某个或多个armeabi版本的SO，那么在APP运行的时候就会报类似的错误："lib_xyz.so" is 32-bit instead of 64-bit

64-bit下使用System.load加载SO："lib_xyz.so" is 32-bit instead of 64-bit

Use 32-bit jni libraries on 64-bit android - Stack Overflow

Found an explanation: 64-bit Android can use 32-bit native libraries as a fallback, 
only if System.loadlLibrary() can't find anything better in the default search path. 
You get an UnsatisfiedLinkError if you force the system to load the 32-bit library using System.load() with the full library path. 
So the first workaround is using System.loadLibrary() instead of System.load().
64-bit处理器可以向下兼容32-bit指令集，即可以运行32-bit动态库，所以APK包仍然可以只保留lib下的一个目录足够（armeabi或armeabi-v7a保留一个），其他目录全部不用配置。

有一种组合错误，就是APK的lib库打的参差不齐，又在64-bit下使用System.load加载SO。
有一个APP在MX5（android5.0.1）下出现了以下异常：

    Caused by: java.lang.UnsatisfiedLinkError: dlopen failed: "/data/data/com.xxx.pris/app_lib/libPDEEngine.so" is 32-bit instead of 64-bit
首先可以大致知道这是一个64位的机器，查看云捕展示的机器信息，确实是arm64-v8a。首先就是看APK的lib目录打的对不对，果然
armeabi下有12个SO，而armeabi-v7a下却只有11个SO。但是他们使用了云捕代码来尝试安全加载SO来降低UnsatisfiedLinkError的异常。

        public static void loadLibrary(final Context context, final String library) {
        if (context == null) {
            throw new IllegalArgumentException("Given context is null");
        }

        if (TextUtils.isEmpty(library)) {
            throw new IllegalArgumentException("Given library is either null or empty");
        }

        try {
            System.loadLibrary(library);
            return;

        } catch (final UnsatisfiedLinkError ignored) {
            // :-(
            CrashHandler.leaveBreadcrumb("ReLinker: System.loadLibrary failed");
        }

        final File workaroundFile = getWorkaroundLibFile(context, library);
        if (!workaroundFile.exists()) {
            unpackLibrary(context, library);
        }

        System.load(workaroundFile.getAbsolutePath());
    }
可以看出，如果因为SO打的参差不齐导致了APK在安装的时候SO就已经有遗漏的没有被安装进lib的加载目录。那么System.loadLibrary的时候便会有异常，然后代码尝试解压并释放所需要的SO文件，但是这个时候只能通过System.load来加载了，又由于当前是arm64-v8a的机器，所以就出现了Use 32-bit jni libraries on 64-bit android - Stack Overflow的问题。

解决办法：

APK包打的时候把SO打的齐全了，并建议只保留一个目录足够（armeabi或armeabi-v7a保留一个）。
云捕SDK在发现上述问题之后，尝试解压释放SO的时候，把解压目录设置到lib的加载路径顺序里去，并继续使用System.loadLibrary来加载（而不是System.load）。并在第一次System.loadLibrary出现异常时，面包屑告诉足够多的信息，例如是否是SO不存在。
java代码混淆导致。
由于Native层需要注册到java层函数，如果java层对应的类名和函数名在打包的时候被混淆了，肯定是会出现异常的。此类问题比较定位解决，但是也比较容易忘记。解决办法就是在proguard混淆时keep掉对应的类和函数。

注册方式不对，或已经被其他类注册。
早期的崩溃捕获功能是在加壳里用的，后来把崩溃捕获的代码单独抽出为云捕SDK，为了保证复用，加壳和云捕SDK共同使用一个libbugrpt.so。外壳如果注册了，则云捕不再注册。如果外壳已经注册过了，云捕仍然要继续注册使用，就会出现上面的错误。解决办法是：当外壳已经注册启用了崩溃捕获，则云捕不再启动。

empty/missing DT_HASH in "libxxxx.so" (built with --hash-style=gnu?)

java.lang.UnsatisfiedLinkError: dlopen failed: empty/missing DT_HASH in "libxxxx.so" (built with --hash-style=gnu?)
at java.lang.Runtime.loadLibrary(Runtime.java:371)
at java.lang.System.loadLibrary(System.java:989)
c++ - Android NDK UnsatisfiedLinkError: "dlopen failed: empty/missing DT_HASH" - Stack Overflow

总结

如果问题出现时可以尝试通过以上的几种方法来排查，如果有其他没有罗列的情形可以发给我，我将会持续收集整理并更新，以期帮助更多的开发者解决问题。
如果想要规避以上的问题，最好的办法就是打好打全相应CPU架构的SO文件。
另外你也可以直接使用云捕SDKRelinker.loadLibrary功能，来帮助你安全加载SO以降低此类UnsatisfiedLinkError的异常。

参考

动态链接库加载原理及HotFix方案介绍 - DEV CLUB
Use 32-bit jni libraries on 64-bit android - Stack Overflow
```
- Error:Cause: org.gradle.api.internal.tasks.DefaultTaskInputs$TaskInputUnionFileCollection cannot be cast to org.gradle.api.internal.file.collections.DefaultConfigurableFileCollection Possible causes for this unexpected error include:
```
Upgrade your gradle build tools to the latest version.

One easy way to do this is to add the latest version of the build tools as a dependency in your build.gradle file, for example:

dependencies {
    classpath 'com.android.tools.build:gradle:2.2.0-beta1'
}
You can then run gradle tasks and gradle will download everything you need.

After Android Studio 2.2 stable released on Sep 19 2016 , the latest version of the build tools is 2.2.0 . So you can fix it by :

dependencies {
    classpath 'com.android.tools.build:gradle:2.2.0'
}
As Android Studio 2.4 stable is not ready to release yet (May 4 2017), the latest stable version of build tools is 2.3.1 .

dependencies {
    classpath 'com.android.tools.build:gradle:2.3.1'
}
If you update this build tools version to 2.3.* , you should also update gradle wrapper version to 3.3 in /yourProjectRoot/gradle/wrapper/gradle-wrapper.properties file. (i know it is not matching question Gradle build failing after update to 3.0, but i strongly suggest you to use latest build tool as google recommended)

BTW: version 2.3.1 of build tool is only exist on jCenter, not MavenCentral, so if you run into error below when run gradlew command line in terminal

Could not find com.android.tools.build:gradle:2.3.1.
 Searched in the following locations:
     https://repo1.maven.org/maven2/com/android/tools/build/gradle/2.3.1/gradle-2.3.1.pom
     https://repo1.maven.org/maven2/com/android/tools/build/gradle/2.3.1/gradle-2.3.1.jar
just replace mavenCentral() with jcenter() like

 repositories {
    jcenter()
    //mavenCentral()
}

```
- [解决com.android.dex.DexException: Multiple dex files define](http://www.cnblogs.com/mengnan/p/4822412.html)
```
出现类似Mutiple dex files这类错误的一般都是有重复的库添加了进去
```
- only the original thread that created a view hierarchy can touch its views
```
http://blog.csdn.net/veryitman/article/details/6384641
```
- Exception in thread "main" java.lang.RuntimeException: Stub!
```
在Java工程中尝试使用Android库中的org.json.JSONObject类，在执行时出现“Stub！”错误，
执行代码
         // Load the requested page converted to a string into a JSONObject.
         JSONObject jsonResponse = new JSONObject(sb.toString());
控制台显示错误：
         Exception in thread "main" java.lang.RuntimeException: Stub!
                at org.json.JSONObject.<init>(JSONObject.java:8)
如另一篇Android工程中无法执行java的main函数相似，Android工程和Java工程还有一定的差异，不能混用他们的库，和函数入口方法。
将上面的代码，移植到在Android工程可以正确执行！
```

- 编码 GBK 的不可映射字符
```
javac -encoding utf-8  xxx.java 指定编码格式
```

- 缺少traceview.bat
```
failed to get the required adt version number from the sdk
	https://drive.google.com/file/d/0BwhcYOlyJN-Pb0lfTnNIQVlXQm8/view
```

- [Subject does not start with '/'.](https://stackoverflow.com/questions/31506158/running-openssl-from-a-bash-script-on-windows-subject-does-not-start-with)
```
The solution is to pass the -subj argument with leading // (double forward slashes) 
and then use  \ (backslashes) to separate the key/value pairs. Like this:

"//O=Org\CN=Name"
This will then be magically passed to openssl in the expected form:

"/O=Org/CN=Name"
So to answer the specific question, you should change the -subj line in your script to the following.

-subj "//C=GB\ST=someplace\L=Provo\O=Achme\CN=${FQDN}"
```