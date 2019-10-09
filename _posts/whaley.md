####账户
- [GitLab_git.moretv.cn](http://git.moretv.cn) ```c@w``` &nbsp;&nbsp;```Cg16```

- 测试DNS ```180.150.189.8```

- [teambition](https://www.teambition.com/projects) ```c@w```&nbsp;&nbsp;&nbsp;```Cw8```

- [bugz](http://bugs.moretv.cn) ```c@w```&nbsp;&nbsp;&nbsp;```Cc16_```

####none
- [gerrit](gerrit.moretv.cn)
```
gerrit提交流程
  git fetch 获取服务器最新文件和代码
  git co origin/分支
  git add .
  git commit -m "日志"    (标题与内容有空行 /changeID前得有空行)   (git commit --amend修改日志)
  git push push_origin(本地定义)  HEAD:refs/for/分支

      config文件add
      [remote "push_origin"]
    	url = ssh://chen.jianliang@gerrit.moretv.cn:29418/wtvos/moretvPlayer
    	fetch = +refs/heads/*:refs/remotes/origin/*
    	push = refs/heads/*:refs/for/*

  网页，gerrit change  添加reviews
```

####工作账户
```
git
http://git.moretv.cn
Cg16

DNS 180.150.189.8  ��


TeamBition   Cw8


bugz Cc123456_


小鹰直播后台 mlive.tvmore.com.cn  moretv@666#3
电视猫 CMS 测试环境 http://testcms.tvmore.com.cn/mtv_cms/login.action  chen.jianliang moretv

播放反馈问题的格式如下
【视频源类型】//腾讯源、自有CDN源、盗链源
【问题的原因是什么】
【引起问题的原因是什么】
【问题后期的解决方法是什么】


小鹰直播

      1. 原因：**********
      2. 解决方案：**********
      3. 哪个版本可验证：**********
      4. 会影响的功能：**********


电视猫
      原因：
      1、腾讯资源转码问题
      解决方案：
      1、腾讯平台同事需要对未播放的视频进行逐个转码
      处理阶段：
      1:已反馈到了腾讯方
      2:腾讯方正在针对多个源进行修复
      3:腾讯反馈并我方验证已修复80%
      4:腾讯已修复完成

      验证版本：
      1:提测时间：xxxx-xx-xx 版本:x.x.x

      2017-04-05
      以修改完成，请测试验证。

http://bus.moretv.cn/admin_moretv/login.html
账户/密码 jianhlijun/hello



http://www.projectviewercentral.com/projectviewer/projectviewerfree.html  mmp




http://vod.tvmore.com.cn/Service/TreeSite?code=program_site           电影树

chen.jianliang@whaley.cn
Cg123456

友盟


Email  Ch8

长连接
  用户名：admin
  密码：whaley@123

  http://lcms.aginomoto.com/
  taiyating
  whaley@123

  测试环境， dns 180.150.189.8



  Hi, 陈建亮
    您的用户名： chenjianliang
    您的部门: 默认
    您的角色： 普通用户
    您的web登录密码： dDacyUb3Y5RWBT2s
    您的ssh密钥文件密码： Kv4plXQXORutPuos
密钥下载地址： http://123.59.83.112:8011/juser/down_key/?id=5172
    说明： 请登陆后再下载密钥！

172.16.17.89   root whaley8888  /data/apps/longconnect/host_service/log/



123789  iPhonep7

gerrit提交流程
  git fetch 获取服务器最新文件和代码
  git co origin/分支
  git add .
  git commit -m "日志"    (标题与内容有空行 /changeID前得有空行)   (git commit --amend修改日志)
  git push push_origin(本地定义)  HEAD:refs/for/分支

      config文件add
      [remote "push_origin"]
    	url = ssh://chen.jianliang@gerrit.moretv.cn:29418/wtvos/moretvPlayer
    	fetch = +refs/heads/*:refs/remotes/origin/*
    	push = refs/heads/*:refs/for/*

  网页，gerrit change  添加reviews



https://tapd.tencent.com/ptlogin/ptlogins/login?site=TAPD&ref=https%3A%2F%2Ftapd.tencent.com%2F

  tfhuang_ex
  Huangtf0021



测试是否是腾讯源
http://vod.aginomoto.com/Service/V3/Program?sid=tvn8b2uwf5a2


PHP管理系统，http://172.16.17.115:8080/index.php?r=site/index



scp -p -P 29418 chen.jianliang@gerrit.moretv.cn:hooks/commit-msg .git/hooks/
git commit --amend


mount -o rw,remount /system




本机
172.16.211.201


172.16.209.174 -1
172.16.208.50   -2



电视猫直接播放
am start -a moretv.action.applaunch --es Data page=play\&sid=tvn8oqmoceop\&contentType=movie

am start -a moretv.action.applaunch --es Data page=play\&sid=tvn8n8tudeu9\&contentType=movie

am start -a moretv.action.applaunch --es Data page=play\&sid=tvn8p8ophivw\&contentType=tv



前端daily build \\172.16.14.28\share\微鲸上海\应用软件\应用开发部\电视猫APK\3.x\316\
			\\172.16.14.28\share\public\TVApp
			
			
			
京东云服务器，/home/cjl    




1.无起播缓冲  startbuffer 




自有cdn源播放，跳集问题

【引起问题的原因是什么】
1.自有cdn源有限流请吴鹏程看下。（e.g. 曾遇到过cdn限流导致播放跳集。）
2.第三方设备系统播放器，系统未知错误。（已经发邮件给moc和李知涛，请帮忙催一下，下一个原因和当前未知错误码有一定关联。）
3.cdn源某ts片缺失（e.g. 蘑菇源 TVB赌城群英会曾遇到过ts片缺失，小米设备上报-10104错误码，进一步播放跳集。）

【问题后期的解决方法是什么】
针对引起问题原因1，减少限流、播放自有节目高峰期同一视频降低视频质量具体需要咨询吴鹏程。

针对引起问题原因2，解决方法。
在处理电视猫播放失败的过程中，小米设备经常发现小米系统自定义的错误Key。属于未定义错误。如果可以的话，需要小米给出相应错误keyCode的具体含义以及如何应对。便于播放器有针对性的修改。   相关bug：Bug 1471198,Bug 2327822 ,Bug 1873862等。     可以的话，此问题需要小米的协助。
	错误片段： 
	1)06-17 17:22:42.085 E/MediaPlayer(15348): error (-10103, 0)
	06-17 17:22:42.090 E/MediaPlayer(15348): Error (-10103,0)
	06-17 17:22:42.090 I/SysMediaPlayer(15348): [MoretvMid-Player onError] what=-10103 extra=0
	06-17 17:22:42.092 E/MediaListPlayer(15348): [MoretvMid-Player onPlayEvent] EVENT_MEDIA_PLAY_ERROR
	2)09-05 20:54:28.657 I/SysMediaPlayer(27432): [MoretvMid-Player onError]     what=-10104 extra=0
	   09-05 20:54:28.658 E/MediaListPlayer(27432): [MoretvMid-Player onPlayEvent] EVENT_MEDIA_PLAY_ERROR
	
	3)07-31 18:35:12.750 E/MediaPlayer( 9960): error (-10104, 0)
	   07-31 18:35:12.760 E/MediaPlayer( 9960): Error (-10104,0)
	   07-31 18:35:12.760 I/SysMediaPlayer( 9960): [MoretvMid-Player onError] what=-10104 extra=0
	   07-31 18:35:12.770 E/MediaListPlayer( 9960): [MoretvMid-Player onPlayEvent] EVENT_MEDIA_PLAY_ERROR
	4)MEDIA_ERROR_UNKNOWN what=-10102 extra=0
	5)MEDIA_ERROR_UNKNOWN  what=-10103 extra=0
	



腾讯ts片
/moviets.tc.qq.com/

project = EAG AND issuetype = Bug AND affectedVersion = 小鹰直播插件化架构重构 ORDER BY key DESC, created DESC
```


#### 电视猫播放器FAQ
```
卡顿
    视频源卡顿

          非缓冲卡顿（无缓冲圈）---设备问题
            播放一定的时长，就卡顿。多次尝试，仍然播放一定时长就卡顿（e.g. 日志信息错位）
            卡死  （e.g. 创维电视 Current Time XXX,一直不变，中间无任何播放器其他信息）

          缓冲卡顿
                解析源---75%解析问题
                腾讯源---
                      有无劫持   key->302
重复播放
    腾讯源     有无劫持  302

硬解有问题 试下软解
网络问题 热点测试

片头片尾
    腾讯源 腾讯传入
    非腾讯源 解析（e.g.跳过片头片尾失败。可能解析问题）


快进，快退问题
  快进，快退后，无法播放。  e.g.(error -38情况下，Error状态下。 获取时间错误。  代码逻辑问题，-38已经是错误状态，获取的时间是不可靠的，建议后期优化)
  快进，快退没有效果。 可能设备系统播放器问题（e.g. do seek to XXX,onSeekComplete之后，Current Time 不是XXX）

闪退
  100分钟节目，却在40等中间时间段收到播放完成通知EVENT_MEDIA_PLAY_COMPLETE。 可能设备系统播放器问题  (e.g.  sk_media_cb_onCompleted)

时间获取错误。

音画不同步
    硬解情况下，可能设备系统播放器问题（e.g. Skipped XXX frames! The application may be doing too much work on its main thread.）
    当前视频过于复杂或者码率过高。支持不够（e.g. MEDIA_INFO_VIDEO_TRACK_LAGGING）
    只有声音，没有画面 可能设备系统播放器问题（ e.g. 长虹电视  pause called in state 16）
    只有声音，画面卡主 可能设备性能不佳 （e.g. 乐米 多处WAIT_FOR_CONCURRENT_GC ）

播放失败
    可能设备系统播放器问题   EVENT_MEDIA_PLAY_ERROR  e.g.（小米设备 what=-10103 extra=0）
    可能设备系统播放器问题   EVENT_MEDIA_PLAY_ERROR  e.g.（小米设备 what=-10102 extra=0）
    可能设备系统播放器问题   EVENT_MEDIA_PLAY_ERROR  e.g.（优朋 what=1 extra=0）
    某节目一直加载，长时间之后才提示无法播放。 e.g. 腾讯视频，网速慢引起。（http[0] is busy）
    超时无法播放 e.g. 腾讯视频，后台文件丢失（多处m3u8 is empty）
    概率性无法播放 e.g. 腾讯视频，后台文件转实时ts问题（304303_230_10011，403）
    某剧集时而无法播放，时而可播放  e.g. 高危节目
    起播开始，seek时间大于视频内容总时间。 e.g. 腾讯会员视频，qq解析获取， 只有五分钟。
    点播或者直播无法硬解播放。 于底层系统播放器而言，当前视频过于复杂或者码率过高。支持不够（e.g. MEDIA_INFO_VIDEO_TRACK_LAGGING）
    腾讯视频播放失败。IP限制或者版权限制导致。 (e.g. 101_80)
    一直缓冲，缓冲速度0   播放准备阶段发生异常未通知前端play error。（代码已修复）
    持续缓冲，无法播放  可能设备系统播放器问题。 （e.g. 多处MEDIA_ERROR_SERVER_DIED媒体服务器挂掉了）
    硬解失败。 (e.g. -1004 MEDIA_ERROR_IO,代码逻辑问题已处理)

直播节目
  1.loading圈每秒种闪一次
    （日志key,MTK_MEDIA_INFO_BUFFERING_START设备系统播放器问题）

signal 具体分析。
  signal 11
  signal 7

crash
    加载安全模块so失败。UnsatisfiedLinkError  部分平台缺失对应的so
    腾讯视频  102,61 vid不合法（可能编辑将专辑id作为剧集id使用）

内存泄漏（播放器销毁时，为释放前端的引用。 注意释放外部引用。）


建议
    UnsupportedOperationException （e.g. 编辑源 playerType设置错误，但是解析出来是需要使用腾讯播放器播放）
    播放腾讯源，传入空url+vid优化
















小米根据已提供的小米播放电视猫问题日志，分析-10103,-10104错误。（-10102类型测试暂未发现）

问题日志摘要：
片段1：
failed to open url(segno 0) http://p2p-gs.tvmore.com.cn/20170927/tvwy9vopk7oq/720p/tvwy9vopk7oq00000.ts?token=2085079212ff5e9668584518707033b7&ts=1509348639539 
The format of 'http://p2p-gs.tvmore.com.cn/20170927/tvwy9vopk7oq/720p/tvwy9vopk7oq_720p.m3u8?token=0633d19d814cce6f05f2489733ba3ad6_0d582913-8b2d-478f-ad97-558244d83941&ts=1509348639539&environment=test 
' cannot be detected. Have a look at the log for details. 

片段2：
10-30 16:40:45.719  1693 19720 E libffmpeg2.7_log: opening url(segno 1) http://p2p-gs.tvmore.com.cn/20170927/tvwy9vopk7oq/1080p/tvwy9vopk7oq00001.ts?token=eeee2f1f08abd1d778b36bdb1baff3c0&ts=1509352829274 
Line 5985: 10-30 16:40:45.719  1693 19720 E libffmpeg2.7_log: user-agent stagefright/1.2 (Linux;Android 6.0) 
Line 6011: 10-30 16:40:45.852  1693 19720 E libffmpeg2.7_log: HTTP/1.1 403 Forbidden 
Line 6012: 10-30 16:40:45.853  1693 19720 W libffmpeg2.7_log: [http @ 0xacdad800] HTTP error 403 Forbidden 

小米结论:10104和10103差不多，都是打不开地址导致的.

自测现象：播放失败后，退出再重新播放，较大概率可正常播放。



结合小米结论、日志摘要以及自测现象，分析小米设备-10103，-10104问题是视频流在某一时刻，系统播放器无法访问。

现需要对比测试，确认视频流是否有问题。
请测试使用不同品牌设备，同时播放同一视频杂警奇兵、超时空男臣、港剧《同盟》。  确认小米出现播放失败时，其他品牌是否播放失败。








【视频源类型】自有CDN源
【问题的原因是什么】播放过程中，发生播放错误
 10-24 16:15:35.465 E/MediaListPlayer(11053): [MoretvMid-Player onPlayEvent] EVENT_MEDIA_PLAY_ERROR
【引起问题的原因是什么】第三方设备，底层系统播放器未知错误。    MEDIA_ERROR_SYSTEM
 10-24 16:15:35.425 E/MediaPlayer(11053): error (1, -2147483648)
 10-24 16:15:35.445 E/MediaPlayer(11053): enter record_logfile : 
 10-24 16:15:35.445 E/MediaPlayer(11053): =====>line 3472
 10-24 16:15:35.445 I/MediaPlayer(11053): =====>line 3484
 10-24 16:15:35.445 I/MediaPlayer(11053): str_log #systemtime:20171024161535
 10-24 16:15:35.445 I/MediaPlayer(11053): url:-1
 10-24 16:15:35.445 I/MediaPlayer(11053): playtime:0
 10-24 16:15:35.445 I/MediaPlayer(11053): status:100
 10-24 16:15:35.445 I/MediaPlayer(11053): desc:Unknow errorPlayer error
  10-24 16:15:35.455 E/MediaPlayer(11053): msg : { when=-31ms what=100 arg1=1 arg2=-2147483648 target=android.media.MediaPlayer$EventHandler }
 10-24 16:15:35.455 E/MediaPlayer(11053): Error (1,-2147483648)
 10-24 16:15:35.465 I/SysMediaPlayer(11053): [MoretvMid-Player onError] what=1 extra=-2147483648
【问题后期的解决方法是什么】



正常播放和异常播放解析获得的地址相同（即非解析问题）
该直播不走代理，中间件播放器直接将解析的地址传给系统播放器。
沈轲提供的日志发现，系统播放器播放时回调错误1_-5013，导致视频无法播放。

所以需要系统排查为什么两次的地址相同，但是一次回调错误、一次可以正常起播。



下面是当时系统出错日志

08-01 19:57:39.372 E/UniPlayerExoImpl( 1367): ####internalError[rendererInitError,],exception:cn.whaley.exoplayer.ParserException: Failed to parse the playlist, could not identify any tags.

08-01 19:57:39.372 E/UniPlayerExoImpl( 1367): 	at cn.whaley.exoplayer.hls.j.c(Unknown Source)

08-01 19:57:39.372 E/UniPlayerExoImpl( 1367): 	at cn.whaley.exoplayer.hls.j.b(Unknown Source)

08-01 19:57:39.372 E/UniPlayerExoImpl( 1367): 	at cn.whaley.exoplayer.upstream.r.aI(Unknown Source)

08-01 19:57:39.372 E/UniPlayerExoImpl( 1367): 	at cn.whaley.exoplayer.upstream.Loader$b.run(Unknown Source)

08-01 19:57:39.372 E/UniPlayerExoImpl( 1367): 	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:422)

08-01 19:57:39.372 E/UniPlayerExoImpl( 1367): 	at java.util.concurrent.FutureTask.run(FutureTask.java:237)

08-01 19:57:39.372 E/UniPlayerExoImpl( 1367): 	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1112)

08-01 19:57:39.372 E/UniPlayerExoImpl( 1367): 	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:587)

08-01 19:57:39.372 E/UniPlayerExoImpl( 1367): 	at java.lang.Thread.run(Thread.java:818)

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): cn.whaley.UniPlayerExoImpl$a.e#L[-1],stack:cn.whaley.exoplayer.ParserException: Failed to parse the playlist, could not identify any tags.

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): 	at cn.whaley.exoplayer.hls.j.c(Unknown Source)

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): 	at cn.whaley.exoplayer.hls.j.b(Unknown Source)

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): 	at cn.whaley.exoplayer.upstream.r.aI(Unknown Source)

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): 	at cn.whaley.exoplayer.upstream.Loader$b.run(Unknown Source)

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): 	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:422)

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): 	at java.util.concurrent.FutureTask.run(FutureTask.java:237)

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): 	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1112)

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): 	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:587)

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): 	at java.lang.Thread.run(Thread.java:818)

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): ,detailMsg=Failed to parse the playlist, could not identify any tags.

08-01 19:57:39.373 D/UniPlayerExoImpl( 1367): cn.whaley.UniPlayerExoImpl$a.b#L[-1],playWhenReady=false, playbackState=idle,PlayerAction=3

08-01 19:57:39.394 E/MediaPlayer( 1367): WhaleyPlayerError:{"Platform":"HELIOSD-02.02.02","BuildDate":"Mon Jul 10 10:04:06 CST 2017","Package":"com.helios.sports","Player":"ExoPlayer","ErrorWhat":"1","ErrorExtra":"-5013","PlayerAction":"Prepare","PlayerUrl":"http:\/\/cfa.live.moguv.com\/live\/cfa_2000.m3u8"}

08-01 19:57:39.394 D/WhaleyPlayerManagerService( 1673): #setEventObject,event=WhaleyPlayerEvent@mainPid[1367],event[2005],arg1[1],arg2[-5013],player[android.media.MediaPlayer@6cc1bdb],packageName[com.helios.sports],service[null],ext[null],calling pid=1367,uid=1000

08-01 19:57:39.395 D/MediaPlayer( 1367): #biErrorMsg=Failed to parse the playlist, could not identify any tags.

08-01 19:57:39.396 I/SysMediaPlayer( 1367): [MiddleWare-player onError] 304302_1_-5013




http://bugs.moretv.cn/buglist.cgi?chfieldfrom=150d


http://bugs.moretv.cn/buglist.cgi?chfieldfrom=150d&list_id=537568






100，天猫魔盒  播放gzip的


TVB跳集、ts片丢失原因

1.高峰cdn限流导致  
系统播放器提前收到播放完成回调

2.后台ts片缺失

3.cdn服务器节点不稳定
日志中可能会有 
MediaPlayer: info/warning (706, 502)  502 Bad Gateway

4.特殊小米设备，-10102,-10103,-10104错误



TVB播放方案
针对提前播放完成：播放器中增加异常提前播放完成、时间点、realurl的BI
针对ts片缺失：增加请求ts片日志(特殊包，强制走代理增加网络日志)
针对cdn服务器节点不稳定（特殊包，强制走代理增加网络日志）
针对小米设备播放错误：



问题处理流程


上次定位ts片丢失问题，在日志中增加m3u8请求





```


SDK相关问题：
1、SDK内的参数配置是否有云端配置，有哪些配置项？
2、SDK的android版本兼容性？是否有已知的兼容问题？是否有已知的存在兼容问题的设备列表？
3、SDK内部是否有闪退信息上报？
4、SDK内部接口域名替换？接口是否有防劫持功能（httpdns方案）
5、视频遮标功能，图片支持能力？
6、SDK播放是否有类似p2p的缓存机制，缓存内存设置情况？是否针对机型、硬件信息不同，配置也不同？低内存设备如何处理
7、SDK接口调用是否有强时序的集成要求，内部是否做好时序保护
8、SDK版本升级频率及升级内容的详细清单（特别是跟播放相关的内容）
9、会员信息SDK鉴权逻辑，SDK信息回调
10、是否存在不在主进程中的UI异步操作


播放相关问题：
1、播放地址劫持
2、播放下载速度慢，CND调度问题
3、占用内存大小问题
4、播放不支持seek问题
5、hevc编码，导致部分设备播放黑屏
6、大小窗切换问题
7、播放错误码列表
8、使用系统播放器还是私有播放器，是否存在部分设备无法解码的兼容性问题
9、会员影片是否有试看正片的功能，流程是怎样的
10、播放鉴权成功及失败流程
11、是否有对某些机型做清晰度适配的能力（比如不提供4K清晰度的选项给低端设备播放）

数据同步问题：
1、 有没有数据对照的方法，如和判断一个节目是优酷没有没有还是我们自己没拿到,有没办法通过名称查到一个节目的showID
2、节目状态变更的实时性是什么样的，假设酷喵晚上八点上线一个节目，一般多久可以体现在增量接口里，节目上下线，多久能同步到电视猫？
3、接口中的taobao.vmac.showpage.get 和?taobao.vmac.show.get 字段差异不大，分别在什么场景下使用
4、接口中的taobao.vmac.videopage.get 和?taobao.vmac.video.get ?字段差异不大，分别在什么场景下使用
5、会员节目付费转免费或者免费转付费，付费转用券或者免付费混排等等的判断逻辑
6、下线节目如何判断

账号及会员相关：
1、明确优酷侧cp账号的登录和下单流程（账号登录是否每次都需要调用优酷登录接口还是只有注册环节需要，下单时是否需要调用登录接口获取用户信息校验用户真实性）
2、退款条件、流程
3、会员权益退、迁移、补偿如何对接
4、后台如何查询用户的的会员权益，包括账号信息、有效期、会员类型等
5、云端账号会员信息数据同步问题
6、优酷优惠活动、会员商品是否会及时同步电视猫，双方价格是否密切保持一致，如何同步给电视猫
7、基于账号查询账号权益，用户权益有效期的接口
8、同一账号是否有多设备登录及播放限制以及其限制规则
9、订单充值接口中的type字段是否为1-ytid充值
10、单片跟正常会员鉴权流程是否一致？是优酷来做还是电视猫来做？



其他问题：
1、 各个业务的对接人，包括sdk、播放器、播控、账号、会员等
2、 测试环境及联调
3、轮播数据信息是否满足现有形态
4、百事通是否有域名、遮标要求，如果有，需要提前跟优酷沟通


广告：
1、广告要有倒计时能力，跳广告购买会员能力？会员关闭广告能力？
2、广告多贴之间以及广告和正片之间的黑场，是否存在设备兼容性问题？
3、广告素材规范、视频格式等
4、支持的广告类型、投放位置
5、广告投放支持的能力（渠道定向、sdk版本定向、节目定向等）
6、提供一个节目可以验证所有的广告形式
7、因为电视猫app在起播放失败时会重试三次，在重试时，优酷sdk是否会再次播放广告，播放重试由谁来做？
8、广告播放信息回调，需要满足现有统计维度（有哪些？）
9、视频播放过程中有中插广告的情况，什么样的视频会有中插广告？会员是否免这样的广告？