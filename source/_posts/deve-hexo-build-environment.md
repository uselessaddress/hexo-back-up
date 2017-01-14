title: Hexo环境搭建
date: 2015-05-30 19:30:22
tags:
- hexo
- git
- SSH
categories: 设计开发
toc: ture
---

### 前言

重装了系统，Hexo没法使用了。第一次安装的时候，没有做任何记录，这次刚好记录一下。

### hexo官网

http://hexo.io/

### 下载安装git
git官方下载地址：http://git-scm.com/download/
分享一个14年版本：http://yunpan.cn/cwqWapRIyFWrA  访问密码 8f1b
下载好之后，双击安装，一路next即可。唯一需要注意的是，在Select Components界面，点选Simple context menu。

### 下载安装Node.js
Node.js官方下载地址：https://nodejs.org/ ，Current version: v0.12.4
分享一个v0.10.31：http://yunpan.cn/cwqtqY8FCpdZc  访问密码 951e
下载好之后，双击安装，一路next即可。

### 安装Hexo
右击任意位置，选择Git Bash Here。执行命令：
`npm install -g hexo`，报错如下：

```
npm ERR! Error: shasum check failed for C:\Users\ADMINI~1\AppData\Local\Temp\npm
-30024-KDJHJzgP\registry.npmjs.org\hexo-cli\-\hexo-cli-0.1.6.tgz
npm ERR! Expected: 7dc3ab939d0889c4bed6a961605ff3e2d67f67a2
npm ERR! Actual:   41de7d67a9b764352eb07c49c32fc38dd7f479b9
npm ERR! From:     https://registry.npmjs.org/hexo-cli/-/hexo-cli-0.1.6.tgz
npm ERR!     at d:\Program Files\nodejs\node_modules\npm\node_modules\sha\index.
js:38:8
npm ERR!     at ReadStream.<anonymous> (d:\Program Files\nodejs\node_modules\npm
\node_modules\sha\index.js:85:7)
npm ERR!     at ReadStream.emit (events.js:117:20)
npm ERR!     at _stream_readable.js:943:16
npm ERR!     at process._tickCallback (node.js:419:13)
npm ERR! If you need help, you may report this *entire* log,
npm ERR! including the npm and node versions, at:
npm ERR!     <http://github.com/npm/npm/issues>

npm ERR! System Windows_NT 6.2.9200
npm ERR! command "d:\\Program Files\\nodejs\\node.exe" "d:\\Program Files\\nodej
s\\node_modules\\npm\\bin\\npm-cli.js" "install" "-g" "hexo"
npm ERR! cwd C:\Users\Administrator\Desktop
npm ERR! node -v v0.10.31
npm ERR! npm -v 1.4.23
npm ERR! registry error parsing json
npm ERR!
npm ERR! Additional logging details can be found in:
npm ERR!     C:\Users\Administrator\Desktop\npm-debug.log
npm ERR! not ok code 0
```
莫非是因为被墙了？换国内镜像源试试。
`npm config set registry="http://registry.cnpmjs.org"`，
然后再次执行`npm install -g hexo`，成功！
<!--more-->
### 创建hexo文件夹
在任意文件夹下（如E:\hexo），打开Git Bash，执行命令：
`hexo init`，
Hexo 即会在目标文件夹建立网站所需要的所有文件。

### 安装依赖包
`npm install`

### 命令缩写
hexo generate = hexo g
hexo server = hexo s
hexo deploy = hexo d
hexo new = hexo n

### 本地查看

执行以下命令：
`hexo g`，
`hexo s`，
然后到浏览器输入`http://localhost:4000`查看效果。
至此，本地博客已经搭建起来了，别人看不到的。

### 部署到GitHub

#### 注册
GitHub官网：http://www.github.com

#### 创建repository
creat new repository，Repository name和自己的用户名相同。比如我的用户名为voidking，那么Repository name就填voidking.github.io。

#### _config.yml
编辑E:\hexo下的_config.yml，修改Deployment部分：
```
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/voidking/voidking.github.io.git
  branch: master
```

#### 部署
`hexo d`，执行该命令，报错：
```
ERROR Deployer not found: git
```
执行命令：
`npm install hexo-deployer-git --save`，再次执行`hexo d`,报错：
```
INFO  Deploying: git
INFO  Clearing .deploy folder...
INFO  Copying files from public folder...
warning: LF will be replaced by CRLF in 2015/05/30/hello-world/index.html.
The file will have its original line endings in your working directory.
......
*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'Administrator@PC-201505290750.(none)')
Username for 'https://github.com': voidking
Password for 'https://voidking@github.com':
error: src refspec master does not match any.
error: failed to push some refs to 'https://github.com/voidking/voidking.github.io.git'
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: error: src refspec master does not match any.
error: failed to push some refs to 'https://github.com/voidking/voidking.github.io.git'

    at ChildProcess.<anonymous> (E:\hexo\node_modules\hexo-deployer-git\node_modules\hexo-util\lib\spawn.js:42:17)
    at ChildProcess.emit (events.js:98:17)
    at maybeClose (child_process.js:756:16)
    at Process.ChildProcess._handle.onexit (child_process.js:823:5)
```
估计必须要先配置SSH，然后才可以上传文件。


#### GitHub之SSH key
1、设置Git的user.name和user.email
在第一次使用Git时，你需要告诉你的协同开发者，你是谁以及你的邮箱，在你提交的时候，Git需要这两个信息。具体通过以下命令设置：
`git config --global user.name "voidking"`
`git config --global user.email "voidking@qq.com"`
需要注意的是，这里的name随意，邮箱是你的联系邮箱，与github完全无关。
查看配置命令：
`git config --list`

2、生成SSH密钥
`ssh-keygen -t rsa -C "voidking@qq.com"`，按3个回车，密码为空。
```
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa):
Created directory '/c/Users/Administrator/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/Administrator/.ssh/id_rsa.
Your public key has been saved in /c/Users/Administrator/.ssh/id_rsa.pub.
The key fingerprint is:
e6:9e:e7:ae:ad:ee:9f:f0:e5:6b:60:63:85:e8:cd:ae voidking@qq.com
```

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

### 再次部署
`hexo d`，根据提示输入用户名和密码，等待一会儿便成功了！
```
INFO  Deploying: git
INFO  Clearing .deploy folder...
INFO  Copying files from public folder...
warning: LF will be replaced by CRLF in 2015/05/30/hello-world/index.html.
The file will have its original line endings in your working directory.
......
[master (root-commit) 7124e90] Site updated: 2015-05-30 23:04:14
 28 files changed, 5746 insertions(+)
 create mode 100644 2015/05/30/hello-world/index.html
 ......
warning: LF will be replaced by CRLF in 2015/05/30/hello-world/index.html.
The file will have its original line endings in your working directory.
......
Username for 'https://github.com': voidking
Password for 'https://voidking@github.com':
Branch master set up to track remote branch master from https://github.com/voidking/voidking.github.io.git.
To https://github.com/voidking/voidking.github.io.git
 * [new branch]      master -> master
INFO  Deploy done: git
```

### 访问测试
访问：http://voidking.github.io/ ，可以看到，Hexo已经搭建成功！

### 参考文档
官方文档：
http://hexo.io/docs/

GitHub Pages
https://pages.github.com/

hexo系列教程：（二）搭建hexo博客
http://zipperary.com/2013/05/28/hexo-guide-2/

Git SSH Key 生成步骤
http://blog.csdn.net/hustpzb/article/details/8230454/

Getting Started - First-Time Git Setup
http://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup

git-scm
http://git-scm.com/