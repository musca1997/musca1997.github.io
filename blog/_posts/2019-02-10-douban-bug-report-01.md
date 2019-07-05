---
title: 可以入侵任何账号的豆瓣安全漏洞（已修复）
key: 20190210
tags: infosec
---


标题党一下，前提条件是得知道对方的登陆手机或者邮箱，所以其实是一个很容易被用来熟人作案或者随便试网上泄露的资料来盗号的漏洞。

前天报告给豆瓣工程师了，今天收到豆邮说 已经修复，所以写下日记。


前几天看了一篇protonmail的漏洞报告，闲着无聊就试了一下在豆瓣上点忘记密码看看是不是有类似的毛病，结果真的找着了。

整个过程正常是这样：**忘记密码--输入手机/邮箱请求验证码--输入验证码（四位）--修改密码**


## 1. 暴力破解手机验证码

考虑到验证码只有四位，如果没有设置错误上限的话其实是可以暴力破解的，所以我就试了一下开多代理多线程，往 `https://accounts.douban.com/j/mobile/reset_password/verify_phone_code` 这个入口发不同组合的四位数字，结果真的在两分钟左右就破出了自己收到的手机验证码。

在验证码提交成功后，response里会有一个随机生成vtoken，使用vtoken往 `https://accounts.douban.com/j/mobile/reset_password/complete` 入口发送修改密码request就能立即修改账号密码。

![](https://cdn.discordapp.com/attachments/447635828496138241/594639889153261582/p57978569.png)

<!--more-->

## 2. 暴力破解邮箱验证码

发送到邮箱的验证码同样也是四位，破解过程和手机差不多，除了入口是 `https://accounts.douban.com/j/mobile/reset_password/verify_email_code` 。

唯一麻烦的一点是大部分人的豆瓣账号同时绑定了手机和邮箱，所以通过了邮箱验证之后会返回一个需要再次认证手机的response如图

![](https://cdn.discordapp.com/attachments/447635828496138241/594639947412275221/p57978473.png)


有意思的一点是，虽然在web界面上到了这一步时手机号是有打一部分马赛克的，但是到了response里，在number的那一栏是明文显示手机号的。说明只要暴力破解邮箱即可得到这个邮箱所对应的手机号码。

有了手机号码之后，重复步骤1即可成功破解并重置密码。

## 反馈

![](https://cdn.discordapp.com/attachments/447635828496138241/594639999799001115/p57982388.png)

