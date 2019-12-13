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

```shell
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