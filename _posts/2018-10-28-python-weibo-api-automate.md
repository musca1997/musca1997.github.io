---
title: python调用微博API自动发微博备忘
key: 20181028
tags: python 微博
---


实现发微博的部分只要一个requests库就行了


## 准备步骤

1. 首先上[微博开发者平台](http://open.weibo.com/)注册一个应用，什么类型都行，只要拿到app key和app secret就行。所以其实也不用验证和提交审核

2. 在高级信息里设置回调URL为 https://api.weibo.com/oauth2/default.html

<!--more-->

## 获取access token


1. 先获取授权，授权机制详见微博授权机制说明

2. 简单说就是访问
`https://api.weibo.com/oauth2/authorize?client_id=这里填入准备工作中获得的app key就行&redirect_uri=https://api.weibo.com/oauth2/default.html` 授权，这时候地址末端就是授权得到的code

```
import requests

url = "https://api.weibo.com/oauth2/access_token"
payload = {
"client_id":"填入app key",
"client_secret":"填入app secret",
"grant_type":"authorization_code",
"code":"填入授权得到的code",
"redirect_uri":"https://api.weibo.com/oauth2/default.html"
}
r = requests.post(url, data=payload)

print(r.text)
```

至此返回的内容就有access token

一般应用创建者本人授权的access token有效期有五年


## 发微博


参考 [微博API](http://open.weibo.com/wiki/API)

现在不管是发文字还是图文都是调用/2/statuses/share.json

直接用requests里的post就行了

```
import requests

url = "https://api.weibo.com/2/statuses/share.json"
payload={
"access_token":"填入上面获得的access token",
"status":"微博内容"
}

files={
"pic":open("test.png", "rb")
}

r = requests.post(url, data=payload, files=files)
```

有一点要注意的是，"status"也即微博的内容必须要带上应用基本信息里的应用地址，否则就只会返回400错误

看了半天写入接口的描述居然是“第三方分享链接到微博”，所以说白了发微博还是要加上一个url，感觉挺不适合私人bot使用场景的

详见 [写入接口](http://open.weibo.com/wiki/2/statuses/share)

用户分享到微博的文本内容，必须做URLencode，内容不超过140个汉字，文本中不能包含“#话题词#”，同时文本中必须包含至少一个第三方分享到微博的网页URL，
且该URL只能是该第三方（调用方）绑定域下的URL链接，绑定域在“我的应用 － 应用信息 － 基本应用信息编辑 － 安全域名”里设置。
