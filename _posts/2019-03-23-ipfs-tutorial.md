---
title: 去中心化文件存储系统IPFS试用
key: 20190323
tags: ipfs
---


**维基百科**

星际文件系统（InterPlanetary File System，缩写IPFS）是一个旨在创建持久且分布式存储和共享文件的网络传输协议。它是一种内容可寻址的对等超媒体分发协议。在IPFS网络中的节点将构成一个分布式文件系统。它是一个开放源代码项目，自2014年开始由Protocol Labs在开源社区的帮助下发展。其最初由Juan Benet设计。

**官网**

https://ipfs.io/

**常见的误解**

有炒币的一般都知道ipfs，但是ipfs不是一种币。ipfs这个系统已经上线了，而filecoin是一个团队想对ipfs节点的贡献者进行奖励的一种数字货币，filecoin还没有上线，如果上线了它的挖矿模式就是作为一个节点，你可以贡献自己的硬盘存储别人的东西，并以此得到filecoin，而别人存储文件也需要支付一定的filecoin。

用大白话来说就是，ipfs这个系统的好处就在于，你传了一个文件上去并被其他人的节点接收到之后，这个文件在这个去中心化的系统上是一直会存在的（因为有许多人都接受并存储了这个文件），而且ipfs会对相同的文件进行去重。

可能因为filecoin还没上线，ipfs上现在的节点还比较少，上传文件后需要等一会才能通过url访问，因为讲白了现在暂时是免费提供存储，估计到了明年就会比较火热了。实际上如果你稍微懂一点计算机也可以建立自己的节点出一份力。


## 如果你是一个没有计算机基础但是想使用这个服务的人

可以试试这个第三方网站 https://globalupload.io/ 

只要直接上传文件就会给你返一个访问地址（就是ipfs.io开头的那个）

不过因为现在节点太少，要过一会才能访问

桌面版： https://github.com/ipfs-shipyard/ipfs-desktop 

浏览器插件版： https://github.com/ipfs-shipyard/ipfs-companion 

## 自己安装初始化

下载安装地址：https://dist.ipfs.io/#go-ipfs

我是在kali linux虚拟机上装的，下载解压后直接

```
cd go-ipfs 
./install.sh 
```

这时候终端输 `ipfs help` 出来了就是成功了

![](https://img3.doubanio.com/view/note/l/public/p59176006.webp)

接下来输入`ipfs init`，就会生成一个identity key，用来识别别人进行连接用的

![](https://img3.doubanio.com/view/note/l/public/p59176061.webp)

接下来就是联网了，`ipfs daemon`，这时候你可以另开一个终端上传文件、下载文件

![](https://img3.doubanio.com/view/note/l/public/p59176130.webp)

上传和下载文件也很简单，`ipfs add [filename]`，`ipfs get [file hash]`

试了一下上传图片，直接从浏览器访问就可以看到 https://ipfs.io/ipfs/QmYkfd45jH9dTGVe2XDJD4RQhTfKv14Hbubv363oBMBrLu

![](https://img3.doubanio.com/view/note/l/public/p59177261.webp)


ipfs还有一个图形化界面，在daemon运行的时候直接访问 `http://localhost:5001/webui` 即可

doc地址： https://docs.ipfs.io/introduction/usage/ 
