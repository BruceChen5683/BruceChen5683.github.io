1. 长短连接，长短轮询

2. rtmp(rtmpt,rtmps,rtmpe,rtmpte,trmfp)
```
解决多媒体数据传输流的多路复用（Multiplexing）和分包（packetizing）的问题。
传输过程中可以根据当前的带宽信息和实际信息的大小动态调整Chunk的大小，从而尽量提高CPU的利用率并减少信息的阻塞机率  Set Chunk Size
```

3. rtmpe

4. rtmps

5. rtp（real-time Transport protocol）

6. rtsp(模拟RTSP请求消息？？？)
```
      一、RTSP协议概述
      RTSP(Real-TimeStream Protocol )是一种基于文本的应用层协议，在语法及一些消息参数等方面，RTSP协议与HTTP协议类似。
      RTSP被用于建立的控制媒体流的传输，它为多媒体服务扮演“网络远程控制”的角色。尽管有时可以把RTSP控制信息和媒体数据流交织在一起传送，但一般情况RTSP本身并不用于转送媒体流数据。媒体数据的传送可通过RTP/RTCP等协议来完成。
      一次基本的RTSP操作过程是:首先，客户端连接到流服务器并发送一个RTSP描述命令（DESCRIBE）。流服务器通过一个SDP描述来进行反馈，反馈信息包括流数量、媒体类型等信息。客户端再分析该SDP描述，并为会话中的每一个流发送一个RTSP建立命令(SETUP)，RTSP建立命令告诉服务器客户端用于接收媒体数据的端口。流媒体连接建立完成后，客户端发送一个播放命令(PLAY)，服务器就开始在UDP上传送媒体流（RTP包）到客户端。 在播放过程中客户端还可以向服务器发送命令来控制快进、快退和暂停等。最后，客户端可发送一个终止命令(TERADOWN)来结束流媒体会话
      二、RTSP协议与HTTP协议区别
      1.  RTSP引入了几种新的方法，比如DESCRIBE、PLAY、SETUP 等，并且有不同的协议标识符，RTSP为rtsp 1.0,HTTP为http 1.1；
      2.  HTTP是无状态的协议，而RTSP为每个会话保持状态；
      3.  RTSP协议的客户端和服务器端都可以发送Request请求，而在HTTPF协议中，只有客户端能发送Request请求。
      4.  在RTSP协议中，载荷数据一般是通过带外方式来传送的(除了交织的情况)，及通过RTP协议在不同的通道中来传送载荷数据。而HTTP协议的载荷数据都是通过带内方式传送的，比如请求的网页数据是在回应的消息体中携带的。
      5.  使用ISO10646(UTF-8) 而不是ISO 8859-1，以配合当前HTML的国际化；
      6.  RTSP使用URI请求时包含绝对URI。而由于历史原因造成的向后兼容性问题，HTTP/1.1只在请求中包含绝对路径，把主机名放入单独的标题域中；
      三、RTSP重要术语
      1.   集合控制(Aggregatecontrol ):
      对多个流的同时控制。对音频/视频来讲，客户端仅需发送一条播放或者暂停消息就可同时控制音频流和视频流。
      2.   实体(Entity)：
      作为请求或者回应的有效负荷传输的信息。由以实体标题域（entity-header field）形式存在的元信息和以实体主体（entity body）形式存在的内容组成
      3.   容器文件（Containerfile）：
      可以容纳多个媒体流的文件。RTSP服务器可以为这些容器文件提供集合控制。
      4.   RTSP会话(RTSP session )：
      RTSP交互的全过程。对一个电影的观看过程,会话(session)包括由客户端建立媒体流传输机制(SETUP)，使用播放(PLAY)或录制(RECORD)开始传送流，用停止(TEARDOWN)关闭流。
```
7. mms（Multimedia Messaging Service，MMS是微软的私有流媒体协议） ```http://www.cnblogs.com/zhengyun_ustc/archive/2005/10/17/mmsprotocol1.html```

8. mmsh

9. icyx

10. httplive

11. udp

12. vlc

13. rtcp
```
    RTCP的主要功能是：服务质量的监视与反馈、媒体间的同步，以及多播组中成员的标识。

    1. AMF
    　　AMF(是Action Message Format的缩写)是在flash和flex中与远程服务端交换数据的一种格式.它是二进制格式，Flash应用与服务端或数据库通过RPC交换数据时，通常都采用这种格式。AMF 1 诞生于Flash Player6，发展到现在已经变成了了AMF3

    2. RTMP
    　　RTMP是Real-Time Messaging Protocol(实时消息传送协议)的缩写，它是Adobe Systems公司为Flash播放器和服务器之间音频、视频和数据传输开发的协议。这是一个标准的，未加密的实时消息传递协议，默认端口是1935，如果未指定连接端口，那么flash客户端会尝试连接其他端口，其尝试连接顺序按照下列顺序依次连接：1935、443、80(RTMP), 80(RTMPT).RTMP协议是被Flash用于对象,视频,音频的传输.该协议建立在TCP协议或者轮询HTTP协议之上。　　

    3. RTMPT
         RTMP的变种，此协建立在HTTP协议之上，是通过HTTP封装后的RTMP协议，默认端口80.
    4. RTMPS
    　　RTMP的另一个变种，此协议是通过SSL加密的RTMP协议，为数据通讯提供安全支持。SSL(Secure Sockets Layer 安全套接层)是为网络通信提供安全及数据完整性的一种安全协议。SSL在传输层对网络连接进行加密。默认端口443。
    5. RTMPE
         RTMP的变种，RTMPE是一个加密版本的RTMP，和RTMPS不同的是RTMPE不采用SSL加密，RTMPE加密快于SSL,并且不需要认证管理。如果没有指定RTMPE端口，Flash播放器将像RTMP协议一样依次扫描下列端口：1935(RTMPE) 443(RTMPE) 80(RTMPE) 80(RTMPTE)
    6. RTMPTE
    　　RTMPTE 这个协议是一个通过加密通道连接的RTMPE，默认端口80.
    7. RTMFP
    　　RTMFP是Adobe公司开发的一套新的通信协议，该协议可以让使用Adobe Flash Player的终端用户之间进行直接通信。此方案提升了目前Flash Player在网络交互方面的体验。RTMFP将减少直播、实时聊天方案的带宽消耗，例如音视频聊天和多人游戏。因为RTMFP的数据在终端用户之间流动，而不是和服务器，所以此方案很适合于大范围的部署。RTMFP因为采用了UDP也提升了传送的速度。UDP是Internet上一种更有效传送音频视频的方法，虽然会有一些丢包，错包。RTMFP有两个特性可以帮助解决一些连接错误。 　　快速连接恢复：连接在以外情况下将快速恢复。例如，一个无线连接掉线了，一旦重连，他将迅速拥有所有的传送能力。 　　IP动态化：一个活动的网络会话将以PEER来标识，即使他变了一个IP，也可以保持原来的会话。例如，一个笔记本在一个无线网络获得了一个新IP地址，他将立刻继续刚才的会话。 　　RTMP和RTMFP之间的不同，最根本的是他们在网络上采用的协议。RTMFP是基于UDP的，RTMP是基于TCP的。UDP在传送直播数据方面比TCP还是有较多优势的，比如减少延时，对丢包的容忍，虽然在可靠性上有所损失。RTMFP支持Flash Player直接发送数据给另一个，而不经过Server，服务端连接将被用来初始化并交互一些客户端之间的信息，也可用来进行服务端调用或者作为进入其他系统的网关。
```
14. ftp

15. ftps

16. sftp(ssh)