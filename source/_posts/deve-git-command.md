title: 常用git命令
toc: true
date: 2015-07-21 09:00:00
tags: git
categories: 设计开发
---
# 前言
学习工作中，越来越习惯使用git，本文记录一下常用的git命令，方便以后查阅。

<!--more-->

# 配置
1、全局配置
```
git config --list
git config --global user.name "voidking"
git config --global user.email "voidking@qq.com"
```

2、生成ssh密钥
`ssh-keygen -t rsa -C "voidking@qq.com"`，按3个回车，密码为空。

在`C:\Users\Administrator\.ssh`下，得到两个文件id_rsa和id_rsa.pub。
需要注意的是，命令中的-C参数，后面跟的内容是注释。也就是说，内容随意，与github完全无关。

3、在GitHub上添加SSH密钥
打开id_rsa.pub，复制全文。https://github.com/settings/ssh ，Add SSH key，粘贴进去。

4、测试
`ssh git@github.com`，提示：

```
The authenticity of host 'github.com (192.30.252.128)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.252.128' (RSA) to the list of known hosts.
Hi voidking! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
```

# 克隆项目
1、普通克隆
```
git clone https://github.com/voidking/voidking.git
```

2、切换分支
```
git branch -a
git checkout origin/branch_name
```

# SSL certificate problem
```
git config --global http.sslVerify false
```

# 上传项目
```
git add .
git commit -m "something"
git push
```

# 版本回退
1、每个文件单独版本回退
```
git status
git log
git reset  1fe37e1bcbb894a1b594cf405ae31880cbaa6cd7 filepath/filename
git checkout filepath/filename
```

2、全部文件版本回退
```
git reset --hard 1fe37e1bcbb894a1b594cf405ae31880cbaa6cd7
```

