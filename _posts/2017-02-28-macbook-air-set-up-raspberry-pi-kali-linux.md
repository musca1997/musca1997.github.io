---
title: 在树莓派3上安装开源游戏模拟器Lakka备忘
key: 20170228
tags: 树莓派
---


Raspberry Pi 3

Macbook Air 系统版本 macOs Sierra 10.12

## 给tf卡写入Kali Linux


**准备步骤**

1. 从Kali Linux官网下载兼容树莓派的镜像：https://www.offensive-security.com/kali-linux-arm-images/

1. 解压出img文件（放在下载文件夹里就行了）

1. tf卡连电脑，用磁盘工具格式化tf卡



**查看tf卡在哪个disk上**

1. 打开 Terminal

1. 输入 `diskutil list`，回车

1. 可以根据disk大小判断出哪个是tf卡的，或者也可以先退出tf卡查看diskutil list再插上tf卡，看看多了哪个就是tf卡了

1. 我的tf卡在disk2

开始写入

1. 在Terminal里，输入：

```
sudo diskutil unmountDisk /dev/disk[n]
```

[n]是所在的数字，比如我的是disk2，所以就是：
```
sudo diskutil unmountDisk /dev/disk2
```

1. 在Terminal输入：
```
sudo dd bs=1m if=~/Downloads/这里写文件名.img of=/dev/rdisk[n]
```

[n]还是数字，Downloads替换成文件夹名字，如果是存在下载文件里就不用改~

我的就是：
```
sudo dd bs=1m if=~/Downloads/kali-2.1.2-rpi2.img of=/dev/rdisk2
```

给了管理员权限之后就开始写入了，这时候什么提示都没有

耐心等到提示已经写入完成

1. ```
sudo diskutil eject /dev/disk2
```

写入完成后在终端输入这个弹出tf卡就行了，disk2的名字记得替换成自己的

1. 写入到此就结束了。拔出tf卡，装入树莓派，接上屏幕，启动。

初始用户名是：root

初始密码是：toor

进入图形化界面后，右上角可以连接wifi

如果想进入文字界面，ctrl+alt+f1

最后一步，更新最新版的kali：
```
sudo apt-get update
```

（一定要注意是不是所有都提示update成功了，因为在国内用国外的源不太稳定，如果不成功可以再试一次，或者修改源的地址）

一些可能比较重要的初始配置
其实我还没设置完，先记一些，接下来还有乱七八糟的就接着补上。

· 修改管理员密码：

在终端输入passwd就可以修改了

· 设置远程控制：

有这一步是因为我没有多余的键盘和鼠标了，所以就直接用mac远程连接树莓派进行控制

mac上安装的是vnc viewer

1. 在树莓派上安装tightvncserver：

在终端输入
```
sudo apt-get install tightvncserver
```

1. 修改vnc控制密码：
```
passwd tightvncserver
```
1. 启动树莓派端vnc：
```
sudo tightvncserver :1
```
就会提示成功了，第一次一般线路是1

1. 查看树莓派IP地址，连接：

直接进入自己家的路由器设置界面看树莓派的ip地址

在mac端的vnc viewer上建立一个控制端，输入树莓派的ip地址，后面加上 ":1" （不包括双引号，没有空格）

至此，就可以在mac上远程操作树莓派了。

但是这个时候问题又来了，现在得到的vnc画面是很简陋的，和树莓派hdmi接口输出的画面是不一样的，这个vnc只是一个类似于shell的功能，可以控制终端等等但是和连接了树莓派的显示屏上的画面上是不同步的。

所以我就去群里问了这个问题，有人给了一个教程：

[画面同步](http://etrd.org/2017/02/20/%E4%BD%BF%E7%94%A8VNCviewer%E8%BF%9C%E7%A8%8B%E8%AE%BF%E9%97%AE%E6%A0%91%E8%8E%93%E6%B4%BE%E7%9A%84HDMI%E8%BE%93%E5%87%BA%E6%A1%8C%E9%9D%A2/)

照着设置了一下，可以同步界面了。

**相关设置**
[shadowsocks](http://www.freebuf.com/sectool/123931.html)
[安装中文字库](http://shumeipai.nxez.com/2016/03/13/how-to-make-raspberry-pi-display-chinese.html)
[kali linux 完整版](https://www.youtube.com/watch?v=3C-TOBsgHME)
[搭建nas](http://shumeipai.nxez.com/2013/08/24/install-nas-on-raspberrypi.html)
