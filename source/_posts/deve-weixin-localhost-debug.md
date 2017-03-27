---
title: 微信本地调试
toc: true
date: 2016-07-13 17:01:16
tags:
- 微信
- 调试
- 内网穿透
categories: 设计开发
---
# 前言
在微信开发的时候，需要填写与微信服务器相连接的url，这个url必须是公网域名。也就是说我们需要在这个公网域名对应的公网服务器上做开发，而没办法本地开发调试。

理想的解决办法，是把公网域名绑定到本地服务器，那就可以本地开发调试了。而本地主机是没有公网IP的，怎么办？动态域名解析。

<!--more-->

# ngrok

> I want to expose a local server behind a NAT or firewall to the internet.
> 我想发布一个能够被公网访问的本地服务。

ngrok官网上的这句话，也正是我们的需求。

按照官方教程，小编测试了一下。ngrok会分配给我们一个动态域名，也可以通过公网访问这个域名（也就访问到了本地服务器），nice。但是，如果想要使用固定域名，必须付费。基础版5dollar一个月，约合人民币33.4元，值得。

# natapp

> 开启您的内网穿透之旅。

natapp，实际上是中国的ngrok。收费更便宜，基础版5元一个月，真心实惠，强烈推荐。

值得一提的是，natapp的服务人员态度很好。由于官方文档还不是特别完善，小编遇到了一个解决不了的问题：付费后依然无法使用固定域名。给服务人员发邮件，很快就回复，还帮忙远程。最后确定是win8.1系统的问题，无法使用配置文件，必须手输参数。

win8.1下natapp启动步骤：在natapp.exe所在的文件夹，鼠标右键+shift，在此处打开命令窗口，输入以下命令即可。
```
natapp -authtoken=xxxxxx
```

# 花生壳

花生壳的业务有点多，其中一个是提供动态域名解析。
小编花了5元在花生壳买了个固定域名，不过没有继续测试，感兴趣的小伙伴自行百度。

# 后记
最终，小编决定长期使用natapp。natapp官网分配的固定域名为`voidking.vip.natapp.cn`，小编绑定了自己的域名`wx.voidking.com`。

在微信公众平台测试号上，JS接口安全域名修改为`wx.voidking.com`，网页授权获取用户基本信息修改为`wx.voidking.com`。

完成了以上步骤，就可以进行微信公众号网站的本地开发了。如果要做微信公众号后台，接口配置信息也需要修改。

# 书签
微信开发如何做本地调试
https://www.zhihu.com/question/25456655

ngrok - secure introspectable tunnels to localhost
https://ngrok.com/

NATAPP 基于ngrok高速内网穿透服务
https://natapp.cn/

花生壳官网
http://www.oray.com/

微信公众平台开发者文档
http://mp.weixin.qq.com/wiki/home/index.html

微信公众平台接口调试工具
http://mp.weixin.qq.com/debug/

微信web开发者工具
http://mp.weixin.qq.com/wiki/10/e5f772f4521da17fa0d7304f68b97d7e.html