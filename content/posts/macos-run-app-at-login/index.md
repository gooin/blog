---
title: "MacOS 手动添加自启动APP"
date: 2022-01-06
draft: false
description: ""
tags: ["others", "tools"]
---

## 问题：
安装了`Alfred`这个非常好用的APP，但是用了一段时间发现一个很难受的地方，明明我在软件中配置了开机自启动，但是开机后并没有启动。

配置如图：
![](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/uPic/20220106_LwMnAN.png)

每次用的时候，突然发现没有启动，就很难受，今天研究了一下解决了。


## 解决

打开 `~/Library/LaunchAgents` 目录，会看到一些已经添加到启动项到一些APP，手动添加一条就可以了。sr
这里参考这个`com.jetbrains.toolbox.plist` 来添加

```shell
$ cd ~/Library/LaunchAgents
$ ll
total 80
-rw-r--r--  1 zhitao  staff   576B  2  8  2021 com.DigiDNA.iMazing2Mac.Mini.plist
-rw-r--r--  1 zhitao  staff   537B  7  8 20:29 com.jetbrains.AppCode.BridgeService.plist
-rw-r--r--  1 zhitao  staff   711B  1  6 18:24 com.jetbrains.toolbox.plist
-rw-r--r--  1 zhitao  staff   786B  7  8 21:11 com.rogueamoeba.loopbackd.plist
-rw-r--r--  1 zhitao  staff   566B  1  6 17:43 com.tencent.Lemon.trash.plist
-rw-r--r--  1 zhitao  staff   862B  1 14  2021 com.valvesoftware.steamclean.plist
-rw-r--r--  1 zhitao  staff   815B  1 10  2021 org.freedownloadmanager.fdm6.plist
-rw-r--r--@ 1 zhitao  staff   677B  3  5  2021 org.virtualbox.vboxwebsrv.plist

$ cat com.jetbrains.toolbox.plist

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>com.baidu.InputService</string>
	<key>RunAtLoad</key>
	<true/>
	<key>LaunchOnlyOnce</key>
	<true/>
	<key>Program</key>
	<string>/Library/Input Methods/BaiduIM.app/Contents/Resources/enableInput</string>
</dict>
</plist>

```

打开活动监视器，找到`Alfred`,并如图复制APP到路径
`/Applications/Alfred 4.app/Contents/MacOS/Alfred`

![20220106_NJyDcp](https://cdn.jsdelivr.net/gh/crexk/pic-archive@main/uPic/20220106_NJyDcp.png)


`~/Library/LaunchAgents`目录下新建一条`com.alfred4.plist`文件， 填入APP路径，保存，重启即可。
其他软件类似，窗口管理软件`Magnet`的问题也这样解决了。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>com.alfred4</string>
    <key>ProgramArguments</key>
    <array>
       <string>/Applications/Alfred 4.app/Contents/MacOS/Alfred</string>
       <string>--minimize</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
  </dict>
</plist>
```

