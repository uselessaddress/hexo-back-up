title: 迁移博客从gitcafe到coding
toc: true
date: 2016-05-13 10:13:03
tags:
- hexo
- gitcafe
- coding
- 博客
categories: 设计开发
---
# 前言

> GitCafe 已加入 CODING 成为 CODING 的一员，共同打造最适合中国开发者使用的 Git 服务平台！GitCafe 将于 2016年5月31日 停止所有服务，届时您在 GitCafe 的账户资料及所有项目都将被永久删除，请尽快将您的资料和项目迁移至 Coding。

啊嘞，小编的博客就在gitcafe上，免不了又要折腾一下了，下面我们就研究一下hexo托管到coding的方法。

# 项目迁移
首先，注册一个coding账户；然后，按照提示，关联gitcafe账户，选择项目进行迁移。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/gitcafe-to-coding/迁移.jpg)

<!--more-->

# Coding Pages 服务
Coding Pages 服务，是一个支持 jekyll 静态站的服务，也就是我们搭建静态博客需要的服务。
1、进入和用户名相同的项目下（小编用户名为voidking，那么就进入voidking项目），点击Pages。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/gitcafe-to-coding/pages.jpg)

2、开启服务，并且绑定需要的域名。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/gitcafe-to-coding/开启服务.jpg)

3、访问http://voidking.coding.me/voidking ，404错误，正常，因为我们还没有coding-pages分支。

4、点击分支，新建分支，输入名称为`coding-pages`，输入起点。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/gitcafe-to-coding/分支.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/gitcafe-to-coding/创建分支.jpg)

5、分支创建成功，访问http://voidking.coding.me/voidking ，依然404。

6、点击Pages，重新部署。等待十多秒，就可以正常访问了。

# 域名解析
在上一步中，我们已经在coding上绑定了域名。但是，要想通过域名访问，我们还需要在自己的域名服务器上完成解析。以万网为例，解析如下：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/gitcafe-to-coding/域名解析.jpg)
然后，访问http://voidking.com ，http://www.voidking.com ，http://blog.voidking.com ，全部正常。

# 发布
博客可以正常访问了，接下来的问题是，它能不能和之前一样，使用`hexo d`就重新部署呢？试试看。

## 设置_config.yml
原配置如下：
```
deploy:
  type: git
  repository: git@gitcafe.com:voidking/voidking.git
  branch: gitcafe-pages
```
修改如下：
```
deploy:
  type: git
  repository: https://git.coding.net/voidking/voidking.git
  branch: coding-pages
```

## 添加SSH key
进入项目，设置，部署公钥，新建部署公钥。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/gitcafe-to-coding/sshkey.jpg)
复制`C:\Users\Administrator\.ssh\id_rsa.pub`中的内容，粘贴进去即可。
关于密钥的生成方法，参见《Hexo环境搭建》。

## 发布测试
`hexo g`，`hexo d`，根据提示输入用户名和密码，结果如下：
```
$ hexo d
INFO  Deploying: git
INFO  Clearing .deploy folder...
INFO  Copying files from public folder...
[master a7d185c] Site updated: 2016-05-13 11:56:08
 568 files changed, 8448 insertions(+), 4942 deletions(-)
 create mode 100644 2016/05/10/deve-npm-install/index.html
 create mode 100644 2016/05/13/deve-gitcafe-to-coding/index.html
 create mode 100644 archives/2016/05/index.html
 create mode 100644 "categories/\350\256\276\350\256\241\345\274\200\345\217\221/page/2/index.html"
 rewrite page/40/index.html (74%)
 create mode 100644 page/59/index.html
 create mode 100644 tags/bower/index.html
 create mode 100644 tags/coding/index.html
 create mode 100644 tags/node/index.html
 create mode 100644 tags/npm/index.html
 create mode 100644 "tags/\345\215\232\345\256\242/index.html"
Username for 'https://git.coding.net': voidking
Password for 'https://voidking@git.coding.net':
Branch master set up to track remote branch coding-pages from https://git.coding.net/voidking/voidking.git.
To https://git.coding.net/voidking/voidking.git
   f14ee2d..a7d185c  master -> coding-pages
INFO  Deploy done: git
```
访问http://www.voidking.com ，刷新下，再刷新下。。。nice，内容已经更新。可见，`hexo d`命令同样适用于coding。

# 后记
如果过了5月31号，还没有完成迁移，怎么办？参见参考文档的《Coding Pages 介绍》，正常创建项目就可以了。

# 参考文档

Coding Pages 介绍
https://coding.net/help/doc/pages/index.html