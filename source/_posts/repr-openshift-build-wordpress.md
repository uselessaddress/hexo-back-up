title: OpenShift搭建WordPress
date: 2013-11-03 19:56:57
tags:
- OpenShift
- WordPress
categories: 精华转载
---
openshift搭建wordpress

openshift为redhat提供的一个云平台，我们可以用此来搭建自己的个人站点，相关信息大家可以去了解openshift，在此我将介绍自己的搭建方式。

1. 安装cygwin
一个实现在window环境中提供linux操作环境。下载后直接进行安装，必须安装的内容有openssh、ruby、make、gcc、git，其它的内容如vim等则根据自己的需要选择安装。在线安装时选择较近的服务器安装，若安装后需要再添加功能，可直接再次打开安装文件，在相同的目录下安装需要的功能即可。

2. 创建openshift的用户
这个不用多说，大家都懂得。
<!--more-->
3. 安装openshift客户端
首先下载http://rubyforge.org/projects/rubygems进行安装，
命令：$ ruby /setup.rb install
其次安装gem
命令：$ gem install rhc

4. 创建域和应用
创建域
命令：$ rhc-create-domain -n 域名 -l 用户账号即邮箱
输入上面命令后，会要求用户输入用户密码，输入后就创建了域，并且会保存公钥到文件中/home/**/.ssh/libra_id_rsa，此公钥我是在openshift网站管理中需要添加的。补充：域和应用的创建也可以在在openshift网站上根据网页提示一步步的建立。
创建应用
命令：$ rhc app create -a 应用名称 -t php-5.3
我在用命令创建应用时候总是报错，ssh: connect to host www-edwin.rhcloud.com port 22: Connection refused网上有相关的说法是要对.ssh目录下的config文件进行修改，我没有进行这步了，如上所说我是直接通过网页建立了。当域与应用都建立之后访问地址也就出来了http://应用名称-域名.rhcloud.com

5. 发布wordpress
在此对应用的管理是通过git进行的，相关了解资料git。
首先将应用克隆到本地
$ git clone
其中的git地址可以到网站自己的对应的应用中查看，就是一ssh://开头的一串字符。
假如你的应用名称是blog，则此时会在你的当前目录下生产一个blog目录，下面有多个文件，我们主要关注的是php文件夹，进入php目录(cd php)将其中的文件都删除(rm *.php)，然后将下载的wordpress解压复制到此目录下可以按此目录结构php/wordpress/index.php，中间wordpress目录的名称就随你修改了。
其次创建数据库，简单的方式是通过网页进行，操作完成后都会显示出数据库的名称、用户名、加密后的密码、访问地址。
$ rhc-ctl-app -a -l -e add-mysql-5.1
再来就是修改wordpress配置文件中的数据库连接信息了，进入目录wordpress目录，找到 wp-config-sample.php文件，复制一份命名为 wp-config.php，打开文件修改其中的信息：
define(‘DB_NAME’, ‘数据库名称’);
define(‘DB_USER’, ‘用户名’);
define(‘DB_PASSWORD’, ‘数据库密码’);
define(‘DB_HOST’, ‘数据库访问地址’); //如：127.2.21.125:3306
最后发布，回到自己的目录下上传添加的所有文件。
$ git add -A
$ git commit -m “test”
$ git push
至此应用就发布完毕，可以通过openshift提供的地址进行访问，配置wordpress即可。

参考：
http://hi.baidu.com/liuhangbin/blog/item/0b602edd2926d1bfcd1166c0.html
http://blog-mking.rhcloud.com/category/openshift/
作者：edwin
本文地址： http://blog-website.rhcloud.com/blog/?p=11
版权所有，转载时必须以链接形式注明作者和原出处并保留本声明。

```
MySQL 5.1 database added.  Please make note of these credentials:

       Root User: adminIGPzEGV
   Root Password: 86RuyKxN_3cz
   Database Name: last

Connection URL: mysql://$OPENSHIFT_MYSQL_DB_HOST:$OPENSHIFT_MYSQL_DB_PORT/

You can manage your new MySQL database by also embedding phpmyadmin.
The phpmyadmin username and password will be the same as the MySQL credentials above.

git clone ssh://52773991e0b8cd25d8000088@last-imsnail.rhcloud.com/~/git/last.git/
cd last/

git add .
git commit -m 'My changes'
git push
```
