# ffmpeg
linux 上 安装 ffmpeg  



# 按起止时间切割媒体文件

```
ffmpeg -ss 0 -t 10 -i test.rmvb test.mp4
```

```
ffmpeg -ss 0:0:00 -t 0:30:00 -i input.mp4 -vcodec copy -acodec copy output.mp4
```







# 图片转视频

```
ffmpeg -loop 1 -i bmp.bmp -vf scale=out_color_matrix=bt709 -color_primaries 1 -color_trc 1 -colorspace 1 -t 30 -pix_fmt yuv420p -y -report bmp.mp4
```







# 相同参数的视频文件合并

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







# 不同参数视频文件的合并（帧宽度、帧高度、帧速率）

```shell
ffmpeg -i 1_264.mp4 -i 2_264.mp4 -filter_complex "[0:v] [0:a] [1:v] [1:a] concat=n=2:v=1:a=1 [v] [a]" -map "[v]" -map "[a]" output.mp4
```

将1_264.mp4 和 2_264.mp4 合并为 output.mp4







# 指定输出文件的视频比特率

```shell
ffmpeg -i input.avi -b:v 64k output.avi
```

设置输出文件的视频比特率（video bitrate）到 64 kbit/s







# 指定输出文件的帧率

```shell
ffmpeg -i input.avi -r 24 output.avi
```

强制输出文件的帧率(frame rate)到 24fps







# 指定输入文件帧率和输出文件帧率

```shell
ffmpeg -r 1 -i input.m2v -r 24 output.avi
```

强制输入文件的帧率到 1 fps 且 强制 输出文件的帧率 到 24 fps







## Example

比如

```shell
ffmpeg -ss 0 -t 10 -i test.rmvb test0.mp4
```

这就是指定了 test.rmvb 文件作为 input file，指定 start time 0s 开始，截取 10s, 创建并输出到 test0.mp4 这个文件。

但是上面这个命令它没有说的是 默认转到 .mp4 文件 音频编码将使用 aac（LC）, 视频编码将使用 h264（High）

仔细找，能在控制台里找到下面这样的内容

```shell
Stream mapping:
  Stream #0:1 -> #0:0 (rv40 (native) -> h264 (libx264))
  Stream #0:0 -> #0:1 (cook (native) -> aac (native))
```

就说明了 音视频转换时的映射关系。



如果输入文件音视频编解码和 目标音视频编解码一致，那么可以尝试仅转换多媒体文件的封装格式，这样转化速度会非常快！

`ffmpeg -i movie.avi -c copy -map 0 output.mp4`

-c 等同于 -codec 用来指定编解码器







# TS文件合并

```shell
copy /b 675b4a4a207000000.ts+675b4a4a207000001.ts+675b4a4a207000002.ts 7.ts
```







# TS文件指定视频编解码器、复制音频流转mp4

```shell
ffmpeg -y -i 7.ts -c:v libx264 -c:a copy -bsf:a aac_adtstoasc 7.mp4
```

ts 文件转 mp4 时，需要注意：

-y 指定了覆盖重名文件时不进行询问

-c:v libx264 指定了视频编解码器使用 标准H264

-c:a copy 指定了音频编解码器直接复制输入文件

-bsf:a aac_adtstoasc 指定了使用比特流过滤器 aac_adtstoasc，这个过滤器就是用于 转ts文件到mp4文件的，因为 ts文件和mp4结构不同，ts文件，原名 MPEG2-TS 格式，它的特点就是 视频流的任一片段开始都独立解码，需要把每一个片段的 带的ADTS流抽出来，原理机制见 https://blog.csdn.net/weiyuefei/article/details/68067944







## 转码mp4文件时，指定 音频编解码器 为AAC（Chrome浏览器可直接播放）

```shell
ffmpeg -i 半路枪手.mp4 -c:v copy -c:a aac 半路枪手_aac.mp4
```

指定了行为是：编解码对于视频流，进行复制，然后对于音频流的编解码使用 AAC







# ffmpeg 下载m3u8

```shell
ffmpeg -i https://video.twimg.com/ext_tw_video/1143530317296406529/pu/pl/720x720/69ZLvxR5w_0y7mVj.m3u8 demo.mp4
```







# 使用**ffprobe**查看文件流信息

```shell
ffprobe -show_streams demo.mp4
```







# 使用**ffplay**播放音频文件(波形图)

```shell
ffplay -i demo.mp3 -showmode 1
```





# 显示可用的硬件加速方法

```shell
ffmpeg -hwaccels
```





# Windows 下使用显卡（CUDA-nvidia）加速

```shell
ffmpeg -hwaccel cuvid -c:v h264_cuvid -i input.mp4 -c:v h264_nvenc -y output.mp4
```

先解码源文件，在编码输出文件

-hwaccel cuvid 指定使用的硬件加速方法

-c:v h264_cuvid 指定解码器

-c:v h264_nvenc 指定编码器

注：会比CPU输出的文件大，质量稍差（人眼很难区分）





# 转封装

```shell
ffmpeg -i input.mkv -c copy output.mp4
```

在源文件内部编码器与目标输出文件编码器一致的前提下，可以只转换封装格式，这样处理转换速度非常快、非常高效



# RMVB转mp4

指定视频解码器为 rv40，视频编码器为英伟达GPU mp4编码器

```shell
ffmpeg -hwaccel cuvid -c:v rv40 -i 魔发奇缘.rmvb -c:v h264_nvenc -y 魔发奇缘.mp4
```

