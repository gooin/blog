---
title: "Chrome修改flags后崩溃无法启动修改默认配置"
date: 2020-02-02
draft: false
description: ""
tags: ["others"]
---


如果修改 chrome://flags 中的实验性内容，有可能会导致浏览器崩溃，导致无法通过浏览器修改回原来的配置

Windows修改： 
Local State 这个文件
C:\Users\{用户名}\AppData\Local\Google\Chrome\User Data\Local State 

打开后是一个json文件，修改即可

![](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/picgo/20210219100914.png)

@2是关闭， @1是开启
这里我把vulkan开启后浏览器打开一直显示不出界面，修改Local State这个文件为 enable-vulkan@2 就恢复正常了。