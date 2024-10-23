---
title: "ffmpeg的一些使用笔记"
date: 2023-07-12
draft: false
description: "ffmpeg的一些使用笔记"
tags: ["ffmpeg", "tools"]
categories : ["tools"]
---

## 合并 yt-dlp 下载的音视频

```bash
ffmpeg -i INPUTVIDEO.mp4 -i INPUTMUSIC.weba -acodec copy -vcodec copy OUTPUTVIDEO.mp4
```

## 修改码率

```bash
ffmpeg -i Japan.mp4 -vf scale=1920:1080 Japan_1080p.mp4
```

## ffmpeg 截取视频片段

https://www.cnblogs.com/Sabre/p/12432554.html

```bash
ffmpeg  -i 源文件名 -vcodec copy -acodec copy -ss 00:00:10 -to 00:00:15 目标文件名 -y
```

-ss time_off  set the start time offset 设置从视频的哪个时间点开始截取，00:00:10是从频的第10s开始截取

-to 截到视频的哪个时间点结束。00:00:15是到视频的第15s结束。

如果用-t 表示截取多长的时间如 上文-to 换位-t则是截取从视频的第10s开始，截取15s时长的视频。即截出来的视频共15s.

注意的地方是：
如果将-ss放在“-i 源文件名”后面则-to的作用就没了，跟-t一样的效果了，变成了截取多长视频。一定要注意-s的位置。

参数解析
-vcodec copy表示使用跟原视频一样的视频编解码器。
-acodec copy表示使用跟原视频一样的音频编解码器。
-i 表示源视频文件
-y 表示如果输出文件已存在则覆盖。

- 如：
```bash
ffmpeg  -i D:/abc.avi -vcodec copy -acodec copy -ss 00:00:10 -to 00:00:15 D:/abc_clip.mp4 -y
```

## ffmpeg转换 webm 到 mp4
用chrome插件录屏，格式是webm的。

### 快速转换，帧率分辨率下降
```bash
  ffmpeg -i video.webm -preset veryfast video.mp4
```

### 普通转换
```bash
ffmpeg -i video.webm  video.mp4
```
## 压制字幕到视频
```bash
ffmpeg -i infile.mp4 -f srt -i infile.srt -c:v copy -c:a copy -c:s mov_text outfile.mp4
#示例
ffmpeg -i "Interview with Senior JS Developer in 2022.mp4" -f srt -i "Interview withSenior JS Developer in 2022.srt" -c:v copy -c:a copy -c:s mov_text outfile.mp4
```
