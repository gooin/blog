---
title: "bilibili UWP 下载视频重命名工具（Python）"
date: 2021-02-02
draft: false
description: ""
tags: ["tools"]
---

## github 
[https://github.com/gooin/bilibili_video_rename_tool](https://github.com/gooin/bilibili_video_rename_tool)

-----

 - 在 Win10商店中下载【哔哩哔哩动画】

 - 登录后找到视频，点击下载，如图：
   
![20210222_HKOyk8](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/uPic/20210222_HKOyk8.png)

 - 格式选`MP4`,**勾选自动合并** （迫于C盘空间不足，截图选了最低码率，自己下载可以选高一点的）
 
![20210222_HWF2ya](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/uPic/20210222_HWF2ya.png)

 - 等待下载完成，目录结构如下图
    
![20210222_cMC0pQ](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/uPic/20210222_cMC0pQ.png)

![20210222_Si89Zm](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/uPic/20210222_Si89Zm.png)

 - 分析一下文件结构，看到 `.dvi` 文件、分p目录下的`.info`文件，打开看看，其实就是个`json` 文件，包含视频系列的标题、描述等信息
    
![20210222_BGU0yP](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/uPic/20210222_BGU0yP.png)
    
   - 思路有了：
     1. 读取`.dvi` 文件，拿到视频系列标题
     2. 读取`.info`文件,为分p的mp4改名
     3. 整合进新的文件夹
   刚好在学习python，
  ## 用法：
 
   打开`init.py`, 修改成你的下载路径, 然后运行即可(python3)（刚开始学py，代码很烂，大佬勿喷）
   
![20210222_XaJ1Qv](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/uPic/20210222_XaJ1Qv.png)
   
  对于分P很多的视频，用代码重命名还是很方便的~ 接下来就可以用自己喜欢的离线播放器观看了~ 

![20210222_HZPA8T](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/uPic/20210222_HZPA8T.png)


![20210222_U8G56x](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/uPic/20210222_U8G56x.png)


## 注意事项
   
   - 目录记得写成截图上那样，有双斜杠
   - 注意C盘的可用空间，哔哩哔哩客户端默认下载目录在C盘


## reference
https://www.bilibili.com/read/cv1422476/