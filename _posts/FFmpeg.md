1. 使用ffmpeg将mp4切片成ts slice 并生成m3u8命令
```
ffmpeg -i mv.mp4 -codec copy -vbsf h264_mp4toannexb -map 0 -f segment -segment_list out.m3u8 -segment_time 10 out%03d.ts
```