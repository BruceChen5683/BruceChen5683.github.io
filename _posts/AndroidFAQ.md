3. #####http https
```
http
  HttpURLConnection,HttpClient(Apache http)
  都支持HTTPS，流媒体上传下载，并且可配置超时以及支持IPv6和连接池技术。但是因为移动设备的局限性，HttpURLConnection会是比Apache Http更好的选择，因为其API简单，运行消耗内存小，并且具有公开化的压缩算法，以及响应缓存，能更好的减少网络使用，提供运行速度和节省电池。但是也不能否认Apache HttpClient，它有大量的灵活的API，实现比较稳定，少有Bug，可造成的问题就是很难在不影响其兼容性的情况下对其进行改进了。现在Android开发者已经慢慢放弃Apache HttpClient的使用，转而使用HttpURLConnection。
  *****但是对于Android2.2之前的版本，HttpURLConnection具有一个致命的BUG，在响应输入流InputStream中调用.Close()方法将会阻碍连接池，******因为这个BUG，只能放弃连接池的使用，但是Apache HttpClient不存在这个问题，当然Android2.3之后的版本中，HttpURLConnection已经解决了这个BUG，可以放心使用。

https
  SSLContext ctx = SSLContext.getInstance("TLS");
         X509TrustManager tm = new X509TrustManager() {
             @Override
             public void checkClientTrusted(X509Certificate[] chain,
                                            String authType) throws CertificateException {
             }
             @Override
             public void checkServerTrusted(X509Certificate[] chain,
                                            String authType) throws CertificateException {
             }
             @Override
             public X509Certificate[] getAcceptedIssuers() {
                 return null;
             }
         };
         ctx.init(null, new TrustManager[]{tm}, null);
         SSLSocketFactory ssf = new SSLSocketFactory(ctx,SSLSocketFactory.ALLOW_ALL_HOSTNAME_VERIFIER);

```

4. installLocation
```
android:installLocation=["auto" | "internalOnly" | "preferExternal"]
  当我们的程序具有如下行为时我们不应该将程序安装到外部存储介质上
  　　①Service
  　　　　正在运行的服务将被终止,当外部存储介质被重新加载时服务不会被重启.
  　　②Alarm Service
  　　　　闹钟服务将被取消,开发者必须在外部存储介质重新加载后重新注册闹钟服务.
  　　③Input Method Engines
  　　　　输入法将被换成系统输入法,当外部存储介质被重新加载后用户可以通过系统设置来启动我们的输入法
  　　④Live Wallpapers
  　　　　我们的动态壁纸将被替换为默认的动态壁纸.外部存储介质重载后,用户可以更换回来.
  　　⑤Live Folders
  　　　　我们的动态文件夹将被移出.
  　　⑥App Widgets
  　　　　我们的小部件将被移出,通常只有系统重启后我们的小部件才可用.
  　　⑦Account Managers
  　　　　使用AccountManager创建的账户将会消失,直至存储介质被重新加载.
  　　⑧Sync Adapters
  　　　　只有外部存储介质被重新加载时,我们的同步功能才可用
  　　⑨Device Administrators
  　　　　我们的DeviceAdminReceiver将会失效
  　　⑩监听开机结束事件
  　　　　系统会在加载外部存储介质之前发送ACTION_BOOT_COMPLETED广播.因此安装在外部存储介质的程序将不能接受开机广播.
  通常,只要我们没有使用上述的特性,我们就可以将我们的程序安装到外部存储介质上.例如,大的游戏程序.当我们的APK文件有几M大时我们应该认真的考虑是否要将程序移动到外部存储介质上以帮助用户节省内存.
```

5. - [eclipse导入到as,9.png图片问题](http://bbs.csdn.net/topics/390953834?page=1)
```
http://www.jianshu.com/p/97b673198a74

 9-
      我也遇到这样的问题，你可以在Android studio里面直接编辑.9图，Android studio的UI编辑能力比Eclipse要严格得多
      点击show bad patches，如果存在bad patches就编译不过
      你需要在Android Studio里面修改好.9图
    11-
      把 .9图片放到dawable文件夹而不是mipmap文件夹
    14-
      虽然9楼是对的，但是，我的情况是：包含的开源项目里的资源都有一堆不合法PNG，那怎么办？？？你丫的让我改？几百来张点9图叫我改？你怎么不去死。。。。后来我找到一个方法：在build.gradle里添加以下两句：aaptOptions.cruncherEnabled = false     aaptOptions.useNewCruncher = false，就直接添加到buildToolsVersion的下方即可，然后你再看是不是好了！！！！这个是用来关闭Android Studio的PNG合法性检查的，直接不让它检查！！！大家可以试试，我的成功了……真是，看到国内论坛一个个全叫人改图的真是无语。
```

6. ANR
```
 KeyDispatchTimeout  5s
 BroadCastTimeout 10s
 ServiceTimeout 20s
```

7. Thread状态
```
 ThreadState (defined at “dalvik/vm/thread.h “)
 THREAD_UNDEFINED = -1, /* makes enum compatible with int32_t */
 THREAD_ZOMBIE = 0, /* TERMINATED */
 THREAD_RUNNING = 1, /* RUNNABLE or running now */
 THREAD_TIMED_WAIT = 2, /* TIMED_WAITING in Object.wait() */
 THREAD_MONITOR = 3, /* BLOCKED on a monitor */
 THREAD_WAIT = 4, /* WAITING in Object.wait() */
 THREAD_INITIALIZING= 5, /* allocated, not yet running */
 THREAD_STARTING = 6, /* started, not yet on thread list */
 THREAD_NATIVE = 7, /* off in a JNI native method */
 THREAD_VMWAIT = 8, /* waiting on a VM resource */
 THREAD_SUSPENDED = 9, /* suspended, usually by GC or debugger */
```

8. Binder?
```
Binder.getCallingUid()  getCallingPid()...
```

9. [SparseArray和ArrayMap都差不多，使用哪个呢？](http://blog.csdn.net/u010687392/article/details/47809295)
```
假设数据量都在千级以内的情况下：

1、如果key的类型已经确定为int类型，那么使用SparseArray，因为它避免了自动装箱的过程，如果key为long类型，它还提供了一个LongSparseArray来确保key为long类型时的使用

2、如果key类型为其它的类型，则使用ArrayMap
```

10. 编译，反编译
```
  apktool d -f [-s]  xxx.apk  xxx_dir反编译生成xxx_dir目录

  apktool b xxx_dir -o update.apk   修改文件后，重新打包生成

  将未签名的update.apk文件拷贝到Auto-sign的文件夹下并改名为update.zip     auto-sign
```

11. adb 连接失败
```
adb server version (31) doesn't match this client  360端口占用
```

12. Failed to create MD5 hash for file
```
检查发现app module下的build.gradle文件编译了不存在的文件：xxx.jar
```

13. Assets
```
Assets打开文件夹中的文件  asserter.open("dir/" + file);
```

14. [深入Android渲染机制](https://www.jianshu.com/p/1ef2a9e5aa91)
```
Android的界面能用png最好是用png了，因为32位的png颜色过渡平滑且支持透明。jpg是像素化压缩过的图片，质量已经下降了，再拿来做9path的按钮和平铺拉伸的控件必然惨不忍睹，要尽量避免。
对于颜色繁杂的，照片墙纸之类的图片（应用的启动画面喜欢搞这种），那用jpg是最好不过了，这种图片压缩前压缩后肉眼分辨几乎不计，如果保存成png体积将是jpg的几倍甚至几十倍，严重浪费体积。


************************当背景无法避免,尽量用Color.TRANSPARENT***********************************

因为透明色Color.TRANSPARENT是不会被渲染的,他是透明的.

//优化前
//优化前: 当图片不为空,ImageView加载图片,然后统一设置背景
Bean bean=list.get(i);
 if (bean.img == 0) {
            Picasso.with(getContext()).load(bean.img).into(holder.imageView);
        }
        chat_author_avatar.setBackgroundColor(bean.backPic);

//优化后
//优化后:当图片不为空,ImageView加载图片,并设置背景为TRANSPARENT;
//当图片为空,ImageView加载TRANSPARENT,然后设置背景为无照片背景
Bean bean=list.get(i);
 if (bean.img == 0) {
            Picasso.with(getContext()).load(android.R.color.transparent).into(holder.imageView);
            holder.imageView.setBackgroundColor(bean.backPic);
        } else {
            Picasso.with(getContext()).load(bean.img).into(holder.imageView);
            holder.imageView.setBackgroundColor(Color.TRANSPARENT);
        }
```

15. compileDebugJavaWithJavac - is not incremental  找不到符号
```
删除import  XXX.XXX.R
```

16. INSTALL_FAILED_OLDER_SDK 
```
设备的Android版本过低，app不支持。 改正的办法就是在程序的Manifest文件（或build.gradle）中将最低版本号调高。
```

17. [RecyclerView只显示一行数据](http://blog.csdn.net/qq_36009027/article/details/70229989)
```
1.把RecyclerView 依赖包版本降低

2.一般我们的onCreateViewHolder是这么写的
public UsersHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    UsersHolder holder =new UsersHolder(LayoutInflater.from(mContext).inflate(R.layout.item_users,parent,false));
    return holder;
}
然后人家解决方案是把他改成：
public UsersHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    UsersHolder holder =new UsersHolder(LayoutInflater.from(mContext).inflate(R.layout.item_users,null,false));
    return holder;
}
也就是把第二个参数设置为null；

item布局高度不能是match_parent. 原来是RecyclerView 他不像ListView你就算match_parent也会自动适应，


```

16. [android optical bounds](https://stackoverflow.com/questions/11406521/android-9-patch-tool-what-is-the-new-layout-bounds-feature)

17. kill process
```
TODO
```

18. NetworkOnMainThreadException
```
Class Overview  
The exception that is thrown when an application attempts to perform a networking operation on its main thread.  
  
This is only thrown for applications targeting the Honeycomb SDK or higher. Applications targeting earlier SDK versions are allowed to do networking on their main event loop threads, but it's heavily discouraged. See the document Designing for Responsiveness.  
  
Also see StrictMode.  
从SDK3.0开始，google不再允许网络请求（HTTP、Socket）等相关操作直接在主线程中，其实本来就不应该这样做，直接在UI线程进行网络操作，会阻塞UI、用户体验不好。
也就是说，在SDK3.0以下的版本，还可以继续在主线程里这样做，在3.0以上，就不行了。
```

19. 自定义View时
```
需要带AttributeSet参数的构造函数
```

- 编译正常，运行错误 NoClassDefFoundError
```
确认相应的类的jar是否存在！
```

- java.io.IOException: read failed: EBADF (Bad file number)
```
OutPutStreamWriter write之后，需要flush
```

20. This file can not be opened as a file descriptor; it is probably compressed
```

```