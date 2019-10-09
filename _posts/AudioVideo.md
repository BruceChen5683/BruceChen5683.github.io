1. NV21/NV12/YV12/YUV420P

2. MediaCodec 流控. 
```
流控就是流量控制。为什么要控制，因为条件有限！涉及到了 TCP 和视频编码：
对 TCP 来说就是控制单位时间内发送数据包的数据量，对编码来说就是控制单位时间内输出数据的数据量。
TCP 的限制条件是网络带宽，流控就是在避免造成或者加剧网络拥塞的前提下，尽可能利用网络带宽。带宽够、网络好，我们就加快速度发送数据包，出现了延迟增大、丢包之后，就放慢发包的速度（因为继续高速发包，可能会加剧网络拥塞，反而发得更慢）。
视频编码的限制条件最初是解码器的能力，码率太高就会无法解码，后来随着 codec 的发展，解码能力不再是瓶颈，限制条件变成了传输带宽/文件大小，我们希望在控制数据量的前提下，画面质量尽可能高** 
一般编码器都可以设置一个目标码率，但编码器的实际输出码率不会完全符合设置，因为在编码过程中实际可以控制的并不是最终输出的码率，而是编码过程中的一个量化参数（Quantization Parameter，QP），它和码率并没有固定的关系，而是取决于图像内容。
无论是要发送的 TCP 数据包，还是要编码的图像，都可能出现“尖峰”，
也就是短时间内出现较大的数据量。TCP 面对尖峰，可以选择不为所动（尤其是网络已经拥塞的时候），这没有太大的问题，但如果视频编码也对尖峰不为所动，那图像质量就会大打折扣了。如果有几帧数据量特别大，但仍要把码率控制在原来的水平，那势必要损失更多的信息，因此图像失真就会更严重。
```

3. Android 硬编码流控 mediaFormat.setInteger(MediaFormat.KEY_BITRATE_MODE, MediaCodecInfo.EncoderCapabilities.BITRATE_MODE_VBR);
```
CQ  表示完全不控制码率，尽最大可能保证图像质量；
CBR 表示编码器会尽量把输出码率控制为设定值，即我们前面提到的“不为所动”；
VBR 表示编码器会根据图像内容的复杂度（实际上是帧间变化量的大小）来动态调整输出码率，图像复杂则码率高，图像简单则码率低；

质量要求高、不在乎带宽、解码器支持码率剧烈波动的情况下，可以选择 CQ 码率控制策略。
VBR 输出码率会在一定范围内波动，对于小幅晃动，方块效应会有所改善，但对剧烈晃动仍无能为力；连续调低码率则会导致码率急剧下降，如果无法接受这个问题，那 VBR 就不是好的选择。
CBR 的优点是稳定可控，这样对实时性的保证有帮助。所以 WebRTC 开发中一般使用的是CBR
```

4. Accessing Raw Video ByteBuffers on Older Devices
```
The size of the video frame (before rotation) can be calculated as such:

MediaFormat format = decoder.getOutputFormat(…);
 int width = format.getInteger(MediaFormat.KEY_WIDTH);
 if (format.containsKey("crop-left") && format.containsKey("crop-right")) {
     width = format.getInteger("crop-right") + 1 - format.getInteger("crop-left");
 }
 int height = format.getInteger(MediaFormat.KEY_HEIGHT);
 if (format.containsKey("crop-top") && format.containsKey("crop-bottom")) {
     height = format.getInteger("crop-bottom") + 1 - format.getInteger("crop-top");
 }
```

5. 视频格式
```
YUV420（NV12、NV21、I420、YV12）
```

