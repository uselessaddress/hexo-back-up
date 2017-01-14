title: 微擎系统搭建
toc: true
date: 2016-01-10 20:20:54
tags:
- 微信
categories: 设计开发
---
# 前言
时隔一年半，再次接触微信公众平台开发。相比于掌上大学、圈里、微站ABC、图灵机器人、小i机器人、FAQ免费智能问答机器人、V5KF、赛科智能机器人，个人更喜欢模块定制的微擎和捷微，源码在自己手里，想怎么搞怎么搞。
本篇短文，就记录下微擎系统搭建的具体步骤。

<!--more-->

# 准备条件
首先，你要有一个公网服务器，服务器上有PHP和MySQL的环境，官方推荐linux(centOS)+ nginx + php5.3，mysql5.6。其次，你要有远程操作服务器的工具，推荐使用xshell和xftp。最后，你需要从微擎官网下载微擎的源码。

## 服务器
### 云擎
先说国内的，BAE、CAE、JAE、SAE等，上次做微信开发时，它们还是免费的，现在有些开始收费了。

再说国外的，GAE、OpenShift、heroku、appfog、mongolab等，但是国内的访问速度一般，要么直接被墙。其中，OpenShift是我最喜欢的，以前使用WordPress在上面搭建了一个博客。

云擎的用法简单，基本都是建立某个类型的应用，然后把代码部署上去。因为云擎有各种限制，比如PHP版本限制、文件大小限制、访问流量限制等，所以不建议使用。但是，云擎的重点在于免费，或者免费一段时间。做做测试还是可以的，对于我等穷屌丝而言，不失为一种福利。

### 主流服务器
阿里云、腾讯云、亚马逊、西部数据、美团云等，按配置收费，可以根据实际需要和经济能力选择。这种服务器，就可以像本地主机一样随意安装配置了。本次的微擎环境，我们就使用阿里云。

## PHP+MySQL
在linux下配置PHP+MySQL的环境，具体步骤请自行百度。如果觉得麻烦，可以在阿里云购买一个配置好的镜像系统，10元左右。

## 远程工具
xshell，用来远程登录服务器系统（一般是Linux），进行一些配置。
xftp，用来管理服务器上的文件。

## 源码
微擎官网：http://www.we7.cc/ 
以前使用微擎，需要把整个微擎系统的源码下载下来，然后部署到服务器上。现在，只需要下载一个名叫“install.php”的文件就可以了。

# 流程
## 连接服务器
1、打开xshell，文件，新建，输入服务器的ip地址，确定，然后输入用户名和密码，便可以连接到服务器。哇咔咔，看到了黑黝黝的shell界面，congratulations！
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F00.jpg)
2、打开xftp，文件，新建，输入服务器的ip地址、用户名、密码，便可以连接到服务器。
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F01.jpg)

## 查看帮助
通过xftp，下载帮助文件，就可以大致知道自己的服务器的配置。
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F02.jpg)
可以看到，小编的web主目录为/alidata/www，OK，我们进入到/alidata/www目录下，里面有一个default目录。没错，这就是默认的web网站了，虽然里面只有一个index.html。而我们在浏览器地址栏输入主机ip地址，看到的就是这个index.html。

## 配置虚拟主机
如果决定直接在default目录下搭建微擎，这个步骤可以忽略。
很多情况下，我们希望在一个服务器上面搭建多个网站。以Apache为例，我们需要配置`/etc/httpd/conf/httpd.conf`，然后执行命令`service httpd restart`，具体步骤可以借鉴参考文档。
最终结果是，我们配置了一个域名为http://test.voidking.com，对应服务器主机目录为/alidata/www/test。

## 上传源码
通过xftp，把从微擎官网下载的“install.php”上传到default目录下。（配置过虚拟主机的话，就上传到test目录下）
在浏览器访问地址：`ServerName/install.php`，其中，ServerName为ip地址或者自己配置的域名。没有意外的话，可以看到微擎的安装引导页面。至此，成功了一半。
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F03.jpg)

## 环境检查
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F04.jpg)
微擎安装引导，会自动检测你的服务器环境是否符合系统安装的要求，很人性化。我们看到，目录权限有问题。
打开xshell，进入到/alidata/www目录下，`chmod -R 777 test`，给test目录和test目录下所有文件增加读写执行权限。
然后，再次检测，已经没有问题了。
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F05.jpg)

## 系统配置
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F06.jpg)
数据库选项，输入正确的用户名和密码即可，其他无需修改。
管理选项，创建一个管理员账号，微擎安装完成后用来登录。

## 下载文件
系统配置完成后，单击“继续”，微擎系统就会下载需要的文件到test文件夹，并且创建一个名为“we7”的数据库。喝杯咖啡的时间，就可以完成下载。

## 更新系统
用刚才配置的管理员账号登录微擎系统，看上去，一切正常。现在就可以使用了吗？不，在线安装的系统是精简版，必须更新，注意，是必须！一般来说，登录后会有更新提示，点过去即可。

# 测试
## 微信公众号
微信公众号分两种，服务号和订阅号。什么差别呢？
1、服务号只有企业或者团体才能申请，而订阅号申请要求较低；
2、服务号显示在聊天列表页，而订阅号都在聊天列表页的订阅号里面；
3、服务号初始就可以使用自定义菜单，而订阅号需要微博认证同时500人订阅才可以使用自定义菜单（2015年8月起，菜单也开放给订阅号了，但是不能在开发者模式使用，仍需认证）；
4、服务号每月可以推送4条消息，而订阅号可以推送30条。

## 交互原理
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F07.jpg)
被动处理用户的请求。图中的个人/企业服务器，指的就是微擎所在的服务器。

![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F08.jpg)
设置微信服务器，或者主动给用户发推送数据。

## 双向绑定
1、在微擎系统，添加公众号，输入自己的公众号和密码一键获取公众号信息，或者自己填入公众号信息。
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F09.jpg)
最终生成我们需要的URL、Token、EncodingAESKey。
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F10.jpg)

2、在微信公众平台，登录自己的公众号。左边导航栏，开发，基本配置。
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F11.jpg)
其中，URL、Token、EncodingAESKey要和微擎中一致。

## helloworld
在微擎系统中，管理公众号，文字回复，添加基本文字回复。输入规则名称、触发规则、回复内容，保存，提交。
![](http://7oxjrx.com1.z0.glb.clouddn.com/%40%2Fimgs%2Fwe7-start%2F12.jpg)
手机关注自己的公众号，在聊天界面输入“helloworld”，看看返回了什么？“恭喜你进入了一个新的世界！”
微擎系统，至此基本搭建完成，更多好玩的功能，等着你去发掘。


# 后记
在搭建微擎系统的过程中，会遇到各种各样意想不到的错误。卧槽，逗我吗？为什么写教程的家伙没有遇到这种错误！莫方，小编也遇到过各种不懂，各种错误。百度、官网、博客、论坛、QQ群、前辈，总能找到你想要的答案。

# 参考文档
微擎开发文档
http://www.we7.cc/docs/#introduce

阿里云一键安装web攻略
https://bbs.aliyun.com/read/153209.html

公钥和私钥
http://blog.csdn.net/tanyujing/article/details/17348321

在一台服务器上搭建多个网站的方法（Apache版）
https://help.aliyun.com/knowledge_detail/6701386.html

Apache 虚拟主机 VirtualHost 配置
http://www.neoease.com/apache-virtual-host/

DocumentRoot does not exist解决方法
http://blog.csdn.net/zhuoyr/article/details/8393854

微信公众号提交开发者提示token验证失败
http://www.68ecshop.com/article-1656.html

Xshell 启动报缺少msvcp110.dll文件
http://blog.csdn.net/z1154505909/article/details/50474104
