##### 自有源
1. 长虹蓝光黑屏,有声音无画面问题
```
TVB蓝光节目采用的个是High Level 5,长虹不支持Level5格式的视频。
长虹可以播放其他源，蓝光节目。是由于其他源的蓝光节目，视频是High level 4，以及更低等级格式的视频。
```

2. 跳集
```
高峰cdn限流导致
后台ts片缺失
cdn服务器节点不稳定，MediaPlayer: info/warning (706, 502)  502 Bad Gateway
```
3. 卡顿
```
tvb节目对应的播放地址被限流了，导致播放卡顿。
```

##### 腾讯播放器
1. ip,版权限制
```
keyCode:101_80   日志摘要：ip limit/area limit
```
2. 后台文件转实时ts问题
```
keyCode:230_10011
```
3. hls广告卡主，联系腾讯改mp4模式
```
需要提供给腾讯的信息   DV=？&MD=？&BD=？
```
4. 账户登录设备过多
```
MoretvMid-Player onError] 304303_50101_1300094
账户登录设备过多。让用户修改密码，等待解封。解封时间一般一俩周。
```

5. 日志分析
```
[MediaPlayerMgr][SystemMediaPlayer.java]url  获取播放连接
```

##### 系统播放器
```
MEDIA_ERROR_UNKNOWN          1 (0x00000001) Unspecified media player error.
MEDIA_ERROR_SERVER_DIED      100 (0x00000064) Media server died.
MEDIA_ERROR_IO               -1004 (0xfffffc14) File or network related operation errors.
MEDIA_ERROR_MALFORMED        -1007 (0xfffffc11) Bitstream is not conforming to the related coding standard or file spec.
MEDIA_ERROR_UNSUPPORTED      -1010 (0xfffffc0e) Bitstream is conforming to the related coding standard or file spec, but the media framework does not support the feature.
MEDIA_ERROR_TIMED_OUT        -110 (0xffffff92) Some operation takes too long to complete, usually more than 3-5 seconds.
MEDIA_ERROR_SYSTEM           -2147483648 低版本android播放器系统错，（例如Mp4多种编码，H.264，H.263等，而低版本android机器只支持部分编码）
MEDIA_INFO_VIDEO_TRACK_LAGGING    700 (0x000002bc)    The video is too complex for the decoder: it can't decode frames fast enough.
MEDIA_ERROR_NOT_VALID_FOR_PROGRESSIVE_PLAYBACK    200（0x000000c8）The video is streamed and its container is not valid for progressive playback i.e the video's index (e.g moov atom) is not at the start of the file
```
name | keycode
---- | ---
LearnShare | 12
Mike |  32
##### 软解
```
none
```


```

When a MediaPlayer object is just created using new or after reset() is called, it is in the Idle state;

 It is a programming error to invoke methods such as getCurrentPosition(), getDuration(), getVideoHeight(), getVideoWidth(), setAudioAttributes(AudioAttributes), setLooping(boolean), setVolume(float, float), pause(), start(), stop(), seekTo(int, int), prepare() or prepareAsync() in the Idle state for both cases.


isPlaying() can be called to test whether the MediaPlayer object is in the Started state.


Although the asynchronuous seekTo(int, int) call returns right away, the actual seek operation may take a while to finish, especially for audio/video being streamed. When the actual seek operation completes, the internal player engine calls a user supplied OnSeekComplete.onSeekComplete() if an OnSeekCompleteListener has been registered beforehand via setOnSeekCompleteListener(OnSeekCompleteListener).

 reset
 constructed



自动跳过片头

attachAuxEffect

 setAudioAttributes()  In order for the target audio attributes type to become effective, this method must be called before prepare() or prepareAsync().

 setBufferingParams

 setPlaybackParams    This method will change state in some cases, depending on when it's called.

 getTrackInfo

 addTimedTextSource

 MediaPlayer.OnDrmConfigListener
 MediaPlayer.OnDrmInfoListener
 MediaPlayer.OnDrmPreparedListener
 MediaPlayer.OnTimedMetaDataAvailableListener
 MediaPlayer.OnTimedTextListener

 MEDIA_ERROR_NOT_VALID_FOR_PROGRESSIVE_PLAYBACK
VIDEO_SCALING_MODE_SCALE_TO_FIT_WITH_CROPPING

deprecateStreamTypeForPlayback

********************************************************************************************************************************
*********************// The MediaPlayer has moved to the Error state, must be reset!*******************************************
********************************************************************************************************************************

AUDIOFOCUS_LOSS    AUDIOFOCUS_GAIN

```

##### 小米播放器
```
e.g.从自测中发现，以下错误可能是播放器在某一刻打开流失败
keyCode:10102 播放地址打开失败
keyCode:10103 不能识别的文件格式
keyCode:10104 解析媒体文件发生错误
```


##### Whaley
```
-5003 m3u8源格式不正确
```