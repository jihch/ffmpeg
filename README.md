# ffmpeg
linux 上 安装 ffmpeg  



按起止时间切割媒体文件

```
ffmpeg -ss 0:0:00 -t 0:30:00 -i input.mp4 -vcodec copy -acodec copy output.mp4
```



