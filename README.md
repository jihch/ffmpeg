# ffmpeg
linux 上 安装 ffmpeg  



按起止时间切割媒体文件

```
ffmpeg -ss 0:0:00 -t 0:30:00 -i input.mp4 -vcodec copy -acodec copy output.mp4
```



图片转视频

```
ffmpeg -loop 1 -i bmp.bmp -vf scale=out_color_matrix=bt709 -color_primaries 1 -color_trc 1 -colorspace 1 -t 30 -pix_fmt yuv420p -y -report bmp.mp4
```



视频合并

```
ffmpeg -f concat -i list.txt -c copy concat.mp4
```



list.txt文件示例
```shell
file 01.mp4
file 02.mp4
file 03.mp4
file 04.mp4
file 05.mp4
file 06.mp4
file 07.mp4
file 08.mp4
file 09.mp4
file 10.mp4
file 11.mp4
file 12.mp4
file 13.mp4
file 14.mp4
file 15.mp4
file 16.mp4
file 17.mp4
```



设置输出文件的视频比特率（video bitrate）到 64 kbit/s：

```shell
ffmpeg -i input.avi -b:v 64k output.avi
```



强制输出文件的帧率(frame rate)到 24fps：

```shell
ffmpeg -i input.avi -r 24 output.avi
```



强制输入文件的帧率到 1 fps 且 强制 输出文件的帧率 到 24 fps：

```shell
ffmpeg -r 1 -i input.m2v -r 24 output.avi
```

ts合并：
```shell
copy /b D:\input1.ts+D:\input2.ts+D:\input3.ts output.ts
```shell
