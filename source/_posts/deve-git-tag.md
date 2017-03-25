title: "git tag使用说明"
toc: true
date: 2015-07-21 10:00:00
tags: git
categories: 设计开发
---
# 打标签
所谓打标签，就是对某一时间点上的版本打上标签。
Git使用的标签有两种类型：轻量级的（lightweight）和含附注的（annotated）。轻量级标签就像是个不会变化的分支，实际上它就是个指向特定提交对象的引用。而含附注标签，实际上是存储在仓库中的一个独立对象，它有自身的校验和信息，包含着标签的名字，电子邮件地址和日期，以及标签说明，标签本身也允许使用 GNU Privacy Guard (GPG) 来签署或验证。一般我们都建议使用含附注型的标签，以便保留相关信息；当然，如果只是临时性加注标签，或者不需要旁注额外信息，用轻量级标签也没问题。

<!--more-->

- 轻量标签。创建轻量标签不需要传递参数，直接指定标签名称即可。
`git tag v1.0.0_light`

- 附注标签。参数a即annotated的缩写，指定标签名。参数m指定标签说明，说明信息会保存在标签对象中。
`git tag -a v1.0.0 -m "v1.0.0"`

# 查看本地标签
`git tag`
`git tag -l 'v1.0.*'`
`git show v1.0.0`


# 删除本地标签
`git tag -d <tagname>`

# 切换到标签
`git check <tagname>`

# 给指定的commit打标签
`git log`，我们看到的commit是一长串十六进制数，比如759f084c094a2e3930224b2dad389349e36ab083，我们要用到至少前4位。

`git tag -a v0.0.0 759f -m "v0.0.0"`，而且，每个commit可以打多个标签。

# 分享标签
默认情况下，git push 并不会把标签传送到远端服务器上，只有通过显式命令才能分享标签到远端仓库。
`git push origin <tagname>`
`git push origin --tag`

# 删除服务器端标签
`git push origin :refs/tags/<tagname>`

# 完整例子

打标签的操作发生在我们commit修改到本地仓库之后。
`git add .`
`git commit -m "fixed some bugs"`
`git tag -a 0.1.3 -m "Release version 0.1.3"`

分享提交标签到远程服务器上
`git push`
`git push --tags`
--tags参数表示提交所有tag至服务器端，普通的git push操作不会推送标签到服务器端。

删除本地标签
`git tag -d 0.1.3`

删除远端服务器标签
`git push origin :refs/tags/0.1.3`

# 参考文档
Git 基础 - 打标签
http://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE

git命令之git tag 给当前分支打标签
http://blog.csdn.net/wangjia55/article/details/8793577

git tag简介
http://blog.csdn.net/hudashi/article/details/7664468

tag打标签
http://blog.csdn.net/wh_19910525/article/details/7470850

git tag操作教程
http://blog.csdn.net/waterforest_pang/article/details/9762863
