1. #####ConstrainLayout
```
layout_constraintTop_creator     
app:layout_constraintDimensionRatio
```
layout_constraintRight_toLeftOf以及layout_constraintLeft_toRightOf 居中         
或layout_constraintLeft_toLeftOf以及layout_constraintRight_toRightOf 居中       
或layout_constraintLeft_toRightOf以及layout_constraintLeft_toLeftOf置于内部左侧      
或layout_constraintRight_toRightOf以及layout_constraintRight_toLeftOf置于外部左侧等效layout_constraintRight_toRightOf  
layout_constraintStart_toEndOf view左侧和参照View右侧在一条线上   
app:layout_goneMarginRight（需要有margin配套，如果只设置了app:layout_goneMarginRight没有设置android:layout_marginRight，则无效。（alpha版本的bug，1.0.1版本已经修复） 约束布局gone时，生效
layout_constraintHorizontal_bias(>=0) 左间隔占比，可大于1
layout_constraintCircle 1.1版本新加，2018.03.15当前稳定版本1.0.2    
```
WRAP_CONTENT : enforcing constraints (Added in 1.1)
If a dimension is set to WRAP_CONTENT, in versions before 1.1 they will be treated as a literal dimension -- meaning, constraints will not limit the resulting dimension. While in general this is enough (and faster), in some situations, you might want to use WRAP_CONTENT, yet keep enforcing constraints to limit the resulting dimension. In that case, you can add one of the corresponding attribute:

app:layout_constrainedWidth=”true|false”
app:layout_constrainedHeight=”true|false”

```
MATCH_CONSTRAINT 和layout_constraintWidth_percent、layout_constraintWidth_min、layout_constraintWidth_max组合
Chain CHAIN_SPREAD,CHAIN_SPREAD_INSIDE,CHAIN_PACKED(可结合bias)

2. #####Dispatch
```
事件分发流程  Activity->ViewGroup->View
```

3. #####NestedScrollView
```
???
```

4. RecycleView
```
???http://blog.csdn.net/lmj623565791/article/details/51118836/
https://www.androidhive.info/2016/01/android-working-with-recycler-view/
```

5. [Android Support库——support annotations](http://blog.csdn.net/maosidiaoxian/article/details/50452706)

6. ViewGroup
```
 demo 设置android:layoutMode未看到差异  ？ TODO https://blog.csdn.net/mr_dsw/article/details/50510674
```

7. SparseArray

8. JobScheduler

9. SurfaceView
```
lockCanvas()锁定画布
unlockCanvasAndPost结束锁定画布，并提及改变 以显示内容
Matrix postScale()，计算时注意float转换 
权限未分配，surfaceView加载图片黑屏    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"></uses-permission>
```

10. AudioTrack 其中最大的区别是**MediaPlayer可以播放多种格式的声音文件**，例如MP3，AAC，WAV，OGG，MIDI等。MediaPlayer会在framework层创建对应的音频解码器。而A**udioTrack只能播放已经解码的PCM流**，如果对比支持的文件格式的话则是AudioTrack只支持wav格式的音频文件，因为wav格式的音频文件大部分都是PCM流。AudioTrack不创建解码器，所以只能播放不需要解码的wav文件。
```
在计算Buffer分配的大小的时候，我们经常用到的一个方法就是：getMinBufferSize。这个函数决定了应用层分配多大的数据Buffer。

AudioTrack.getMinBufferSize(8000,//每秒8K个采样点                              
　　      AudioFormat.CHANNEL_CONFIGURATION_STEREO,//双声道                  
        AudioFormat.ENCODING_PCM_16BIT);
从AudioTrack.getMinBufferSize开始追溯代码，可以发现在底层的代码中有一个很重要的概念：Frame（帧）。Frame是一个单位，用来描述数据量的多少。1单位的Frame等于1个采样点的字节数×声道数（比如PCM16，双声道的1个Frame等于2×2=4字节）。1个采样点只针对一个声道，而实际上可能会有一或多个声道。由于不能用一个独立的单位来表示全部声道一次采样的数据量，也就引出了Frame的概念。Frame的大小，就是一个采样点的字节数×声道数。另外，在目前的声卡驱动程序中，其内部缓冲区也是采用Frame作为单位来分配和管理的。

下面是追溯到的native层的方法：

 // minBufCount表示缓冲区的最少个数，它以Frame作为单位
   uint32_t minBufCount = afLatency / ((1000 *afFrameCount)/afSamplingRate);
    if(minBufCount < 2) minBufCount = 2;//至少要两个缓冲
 
   //计算最小帧个数
   uint32_tminFrameCount =               
         (afFrameCount*sampleRateInHertz*minBufCount)/afSamplingRate;
  //下面根据最小的FrameCount计算最小的缓冲大小   
   intminBuffSize = minFrameCount //计算方法完全符合我们前面关于Frame的介绍
           * (audioFormat == javaAudioTrackFields.PCM16 ? 2 : 1)
           * nbChannels;
 
    returnminBuffSize;

getMinBufSize会综合考虑硬件的情况（诸如是否支持采样率，硬件本身的延迟情况等）后，得出一个最小缓冲区的大小。一般我们分配的缓冲大小会是它的整数倍。

：MediaPlayer 更加适合在后台长时间播放本地音乐文件或者在线的流式资源; SoundPool 则适合播放比较短的音频片段，比如游戏声音、按键声、铃声片段等等，它可以同时播放多个音频; 而 AudioTrack 则更接近底层，提供了非常强大的控制能力，支持低延迟播放，适合流媒体和VoIP语音电话等场景。
```