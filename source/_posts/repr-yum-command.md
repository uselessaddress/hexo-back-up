title: yum命令
date: 2013-10-21 20:33:47
tags: 
- Linux
- yum
- 命令
categories: 精华转载
---

### 通过yum安装和删除RPM包
安装rpm包,如dhcp
[root@localhost ~]#`yum install dhcp`

删除rpm包,包括与该包有依赖性的包
[root@localhost ~]#`yum remove licq`
注意:同时会提示删除licq-gnome,licq-qt,licq-text

### 通过yum工具更新软件包
检查可更新的rpm包：
[root@localhost ~]#`yum check-update`

更新所有的rpm包：
[root@localhost ~]#`yum update`
<!--more-->
更新指定的rpm包,如更新kernel和kernel source：
[root@localhost ~]#`yum update kernel kernel-source`

大规模的版本升级,与yum update不同的是,陈旧的淘汰的包也会升级：
[root@localhost ~]#`yum upgrade`

### 通过yum查询RPM包信息
列出资源库中所有可以安装或更新的rpm包的信息：
[root@localhost ~]#`yum info`

列出资源库中所有可以更新的rpm包的信息：
[root@localhost ~]#`yum info updates`

列出已经安装的所有的rpm包的信息：
[root@localhost ~]#`yum info installed`

列出已经安装的但是不包含在资源库中的rpm包的信息：
[root@localhost ~]#`yum info extras`
注：也就是通过其它网站下载安装的rpm包的信息。

列出资源库中所有可以安装或更新的rpm包的信息：
[root@localhost ~]#`yum list`

列出资源库中特定的可以安装或更新以及已经安装的rpm包的信息：
[root@localhost ~]#`yum list sendmail`
[root@localhost ~]#`yum list gcc*`
注意：可以在rpm包名中使用匹配符， 如上面例子是列出所有以gcc开头的rpm包的信息。

搜索匹配特定字符的rpm包的详细信息：
[root@localhost ~]#y`um search wget`
注意：可以通过“search”在rpm包名，包描述中进行搜索。

搜索包含特定文件名的rpm包：
[root@localhost ~]#`yum provides realplay`

### 通过yum操作暂存信息（/var/cache/yum）
清除暂存的rpm包文件：
[root@localhost ~]#`yum clean packages`

清除暂存的rpm头文件：
[root@localhost ~]#`yum clean  headers`

 清除暂存中旧的rpm头文件和包文件：
[root@localhost ~]#`yum clean  all`

### Redhat Linux下用yum升级系统
yum也可以升级Redhat Linux系统，在Redhat Linux系统安装盘中默认没有yum的安装包，由于Redhat Linux与Centos Linux基本一致，因此可以用同版本同内核的Centos Linux的yum包在Redhat Linux上进行安装。安装过程在上面章节已经讲述，这里不在多说。
由于使用的是Centos Linux的yum包在Redhat Linux下进行的安装，因此在Redhat Linux下需要增加资源库，定义yum的非官方库文件，让一些必需的软件包通过yum也能够安装。
首先建立dag.repo，定义非官方库：
[root@localhost ~]#`vi /etc/yum.repos.d/dag.repo`

[dag]
name=Dag RPM Repository for RHEL4
baseurl=http://ftp.riken.jp/Linux/dag/redhat/el4/en/$basearch/dag/
enabled=1
gpgcheck=1

接着导入非官方库的GPG：
[root@localhost ~]#`rpm --import  http://ftp.riken.jp/Linux/caos/centos/RPM-GPG-KEY-centos4`
注意：此步骤很重要，如果没有导入授权的RPM-GPG-KEY，在使用yum升级安装软件时就会提示软件不合法，结合上下文可以看出，在Centos下进行yum配置的时候，并没有涉及到导入RPM-GPG-KEY，那是因为连接的资源库为Centos官方的库，而升级的系统也是Centos，当然无需授 权，而这里我们升级的系统是Redhat Linux，而用的资源文件是Centos的，所以必须导入Centos的RPM-GPG-KEY，系统才认为升级的包是合法的。 

最后，就可以使用非官方定义的rpm包升级系统：
[root@localhost ~]#`yum update`


### 实践
yum install vim
yum install gcc
yum install gcc-c++