title: Android开发——版本控制和代码分享
toc: true
date: 2015-07-18 23:39:27
tags:
- android
- git
categories: 设计开发
---
# 前言
很自然的，觉得自己该使用一些版本控制工具了。虽然以前也使用过，但一直没有系统学习，现在，搞一搞吧！

以《Android开发——帧动画》中的Demo为例。

# git
作为一名程序员，不使用git，都不好意思和别人讲话。git的安装过程自行百度，或者参考小编的《Hexo环境搭建》。

<!--more-->

## 在github新建repository
新建repository，命名为demo。可以看到，repository的地址https://github.com/voidking/demo.git
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/repository.jpg)

## 配置git环境
AS中，Settings，Version Control，Git。配置Path to Git executable，然后Test。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/gitset.jpg)

## git init
AS中，VCS，Import into Version Cotrol，Create Git Repository。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/gitinit.jpg)
OK之后，发现Project里的文件变成了红色。

> git init，其实就是新建了一个本地仓库.git，相当于hexo里的.deploy_git。

## git add
单击Project名Demo，VCS，Git，Add。发现Project里的文件变成了绿色。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/gitadd.jpg)

>git add，其实就是把目标文件快照放入暂存区域，同时未曾跟踪过的文件标记为需要跟踪。

## git commit
VCS，Git，Commit Directory。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/gitcommit.jpg)
单击Commit。

> git commit，提交时记录的是放在暂存区域的快照，任何还未暂存的仍然保持已修改状态，可以在下次提交时纳入版本管理。每一次运行提交操作，都是对你项目作一次快照，以后可以回到这个状态，或者进行比较。

## 指定连接目标
指定本地库与github的repository相连。

方法一（推荐）：
在Demo目录下，右键，Git Bash Here
`git remote add origin https://github.com/voidking/demo.git`
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/gitremote.jpg)

方法二：
VCS，Git，Push。如果没有执行方法一，这时可以看到Define Remote。填入`https://github.com/voidking/demo.git`，会报错，不知道为什么。

```
Remote URL is invalid: unable to access 'https://github.com/voidking/demo.git'
```

## git push
VCS，Git，Push。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/gitpush.jpg)

填入用户名和密码。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/gitpush2.jpg)
之后，可以看到窗口最下面有个push进度条在跑。

如果弹出提示：
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/masterpassword.jpg)
我们勾选Encrypt with OS user credentials，意思是使用本机的SSH key。不明白的地方请见小编另一篇文章，《Hexo环境搭建》。

过一会儿，便可以看到成功的提示！
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/pushsuccess.jpg)

## github
我们查看一下github上的repository。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/frameanimation/success.jpg)
至此，大功告成！

> AS集成了GitHub插件，配置起来比上面的git配置要简单的多，钟爱GitHub的小伙伴不妨百度学习一下。

# svn
在实习的时候，使用过一段时间的svn。个人感觉，svn在团队开发中很好用。但是，当时在局域网使用，有些局限性。如果svn服务器搭在公网服务器上（比如阿里云），应该是极好的。值得一提的是，利用svn向sae的项目中上传文件，非常方便。


## 免费SVN服务器

比起自己搭建服务器，抠门的小编更愿意使用免费svn服务器。但是，这种免费svn服务器极少，找了很久，只找到一个TaoCode。

http://code.taobao.org/

## 安装svn
svn，分为服务器和客户端，因为服务器使用TaoCode，所以我们只安装客户端就可以了。

1、服务器下载地址
https://subversion.apache.org/download/

2、客户端下载地址
http://tortoisesvn.net/downloads.html

3、服务器安装
略。（突然感觉一个略字大气磅礴，瞬间省了好多字）

4、客户端安装
需要注意的是，安装时选择安装command line（默认不安装），为了在AS中使用。

## 在TaoCode新建项目
新建项目，命名为vkdemo。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/version/taocode.jpg)
可以看到，svn repo地址为http://code.taobao.org/svn/vkdemo/ 

## Checkout from Subversion
1、打开AS，VCS，Checkout from Version Control，Subversion。
2、点击加号（Add Repository Location）。
3、添加Repository URL。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/version/checkout.jpg)
此时Repository里啥也没有，咱们就不Checkout了。

## Import into Subversion
1、打开AS，VCS，Import from Version Control，Import into Subversion。
2、选中URL，Import。
3、选中需要Import的目录，OK。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/version/import.jpg)

点击OK，啊嘞，报错了！
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/version/error.jpg)

哥哥是见过大世面的人，这点小错算什么！
Settings，Version Control，Subversion，去掉General选项卡中所有勾。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/version/subversion.jpg)
再次import，成功！
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/version/import2.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/version/success.jpg)
不过，由于是整个项目上传，包括编译文件等，上传过程会很慢。

## 浏览svn文件夹
打开AS，VCS，Browse VCS Repository，Browse Subversion Repository，选择URL。
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/version/browse.jpg)

再看Project目录：
![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/version/state.jpg)
- 文件红色：表示文件没有添加到服务器

- 绿色：表示没有更新新的修改到服务器

- 普通黑色：表示和服务器同步


## 文件夹符号说明

![](http://7oxjrx.com1.z0.glb.clouddn.com/@/imgs/android/version/state.png)

黄色感叹号(有冲突)：这是有冲突了，冲突就是说你对某个文件进行了修改，别人也对这个文件进行了修改，别人抢在你提交之前先提交了，这时你再提交就会被提示发生冲突，而不允许你提交，防止你的提交覆盖了别人的修改。
要解决冲突，如果你确认你的修改是无效的，则用TSVN还原你的修改就行了；如果认为你的修改是正确的，别人的提交是无效的，那么用TSVN先标记为“解决冲突”，然后就可以提交了；如果你认为你的修改和别人的修改都有一部分是有效的，那么你就把别人的修改手动合并到你的修改中，然后使用TSVN标注为“解决冲突”，然后就可以提交了。

米字号(有本地修改代码)：这是说明你有未提交的本地代码。 

问号(新加入的资源)：这说明该文件是项目中新增文件资源，新增资源可以是文件、图片、代码等。

红色感叹号(本地代码与库没有保持一致)：这说明本地代码跟库上没有保持一致，如果用户想修复，可以将带红色感叹号图标文件删除，直接update即可。 

灰色向右箭头(本地修改过)：本地代码没有及时上库。

蓝色向左箭头(SVN上修改过)：记得更新代码后修改，提交前跟svn对比习惯。

灰色向右且中间有个加号的箭头(本地比SVN上多出的文件)：修改完记得跟svn保持一致。

蓝色向左且中间有个加号的箭头(SVN上比本地多出的文件)：删除该文件后，再次更新，将svn上文件全部更新下来。 

灰色向右且中间有个减号的箭头(本地删除了,而SVN上未删除的文件)：也就是说你删除确认后，一定要记得上库，跟svn保持一致 

蓝色向左且中间有个减号的箭头(SVN上删除了,而本地未删除的文件)：比对svn库上代码，确定需要删除后，更新svn(删除无用代码)。

红色双向箭头(SVN上修改过,本地也修改过的文件)：这个表示本地和svn上都修改过，最好就是把本地修改合并到svn，修改代码前最后先更新。

## 单个文件签入签出
右键subversion
Add：添加到服务器

Commit：提交

Update：更新，获取新版本

Integrate：合并

# 后记
对于git和svn的区别，哥只想说——啥，啥，这都是啥！。。。还是稍微总结一下吧！

1、git是分布式的，svn不是。
> svn貌似进化了，它提供的命令`svnsync`就可以checkout所有版本到本地，这是分布式的特点哇！

2、git直接记录快照，而非差异比较。

3、git几乎所有操作都是本地执行（因为区别1）。

知道了这三条，平时吹吹牛逼应该够了，不满足的小伙伴请移步参考资料。

# 参考资料
《Pro Git》
http://git-scm.com/book/zh/v1

Learn Git Branching
http://learngitbranching.js.org/

git-使用简易指南
http://www.bootcss.com/p/git-guide/

Android Studio之版本管理工具Git
http://www.wfuyu.com/technology/22499.html

Android studio -VSN 使用笔记
http://www.cnblogs.com/shaocm/p/4182380.html

GIT和SVN之间的五个基本区别
http://www.oschina.net/news/12542/git-and-svn

Git与svn的区别
http://blog.csdn.net/huacuilaifa/article/details/19124635

为什么说 Git 比 SVN 更好
http://www.oschina.net/news/29214/why-git-is-better-than-svn

分布式和集中式版本控制工具-svn,git,mercurial比较分析
http://blog.csdn.net/augusdi/article/details/29846253
