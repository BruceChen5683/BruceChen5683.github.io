1. Failed to resolve: com.android.support:appcompat-v7:26.0.0
```
To use support libraries starting from version 26.0.0 you need to add Google's Maven repository to your project's build.gradle file as described here: https://developer.android.com/topic/libraries/support-library/setup.html

allprojects {
        repositories {
            jcenter()
            maven {
                url "https://maven.google.com"
            }
        }
    }
For Android Studio 3.0.0 and above:

allprojects {
        repositories {
            jcenter()
            google()
        }
    }
```

2.  The SDK platform-tools version ((23)) is too old to check APIs compiled with API 23
```
更新SDK platform-tools version 
FIle -> Invalidate Caches/Restart
```

3. run-as: Package is not debuggable
```
Device file Explorer app需要是debug版本，才显示/data/data/xxx/files目录
```

4. Could not execute build using Gradle installation/distribution
```
问题原因：中国连接gradle的地址被强了
解决方案：修改android studio设置Http Proxy->Auto-detect proxy settings
```