---
title: 用Macbook Air给树莓派3装Kali Linux系统备忘
key: 20170228
tags: 树莓派
---

![](https://cdn.discordapp.com/attachments/447635828496138241/482847287597334529/p41189429.png)

昨天突然很想买个gameboy怀念童年游戏（有毛病

然后突然想起来之前看到的lakka，想到自己还有一张空的tf卡，觉得刷个这个系统偶尔玩玩还挺好的。

基本上小时候玩的那些游戏，只要能找到rom的都能玩。

Lakka是一个叫libretro的团队开发的，系统内核也叫libretro，Lakka是他们开发的游戏模拟平台RetroArch的linux轻便版。

[libretro 官网](https://www.libretro.com/ )

[lakka 官网](http://www.lakka.tv/)

<!--more-->

[github page](https://github.com/libretro/Lakka)

[download lakka](http://www.lakka.tv/get/linux/)

![](https://cdn.discordapp.com/attachments/447635828496138241/482847371500191744/p41189439.png)

装好之后将tf卡放入树莓派启动

首先去连接wifi，发现只要一进入wifi输密码的界面就会卡住，在他们页面上发现已经有人反映这个问题了，团队给的回复是在下一次发布新版的时候会修复这个bug。如果接的是游戏手柄一般不会发生这种情况，但是如果是用普通的电脑键盘是会这样的。

但是我手头没有手柄，所以就只好用自己的电脑ssh连接树莓派设置wifi

**ssh连接树莓派过程如下：**

1. 先去设置里看看树莓派的ssh是不是开着（一般默认为开着吧但是我的不知道为什么是关闭的），要保持开着

1. 用网线连接树莓派，上路由器查树莓派的ip地址

1. 在自己的电脑终端输入 ssh root@你的树莓派ip地址（比如：ssh root@192.168.1.100 这样的格式）

1. 输入默认密码root，就连接上了

1. 终端输入 `connmanctl`

1. 接下来输入 `enable wifi`

然后 `agent on`

然后 `scan wifi`

然后 `services`

（全都是基于connmanctl命令下的）

接下来就会出现一堆扫描到的wifi的名字

出现的格式是 wifi名字 加上 wifi_一些乱七八糟的

（官网给出的例子是：

`android-device-tether wifi_de65fr56325_524632863278
FBIDrone wifi_je86fe48321_532486348931`）

输入 `connect` 加上想连接的wifi的后面的那段乱七八糟的东西

（官网给出的例子是：

`connmanctl connect wifi_de65fr56325_524632863278`）

反正就是这么个原理……

然后就连上无线了，可以拔掉网线以后开机就自动连上网了

这么折腾过之后，进入文件夹就会发现lakka在共享电脑里了

装游戏也很简单，从自己的电脑进入lakka的文件列表里，把下载好的游戏传到ROMS文件夹里，然后在树莓派端scan一下，就能玩了。

不知道怎么弄可以看：

http://www.lakka.tv/doc/ROMs/

http://www.lakka.tv/get/linux/rpi/install/first-boot/games/
