- intellij idea 编译运行找不到或无法加载主类 
```
指定输出目录和源码一个目录
```

- Could not find com.android.tools.build:aapt2:3.2.1  **add google()**
```
eginning with Android Studio 3.2 Canary 11, the source for AAPT2 (Android Asset Packaging Tool 2) is Google's Maven repository.

To use AAPT2, make sure that you have a google() dependency in your build.gradle file, as shown here:

buildscript {
  repositories {
      google() // here
      jcenter()
  }
  dependencies {
      classpath 'com.android.tools.build:gradle:3.2.0-alpha12'
  }
} 
allprojects {
  repositories {
      google() // and here
      jcenter()
}
```