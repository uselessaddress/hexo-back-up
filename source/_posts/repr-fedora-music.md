title: Fedora播放音乐
date: 2013-11-12 20:58:47
tags: 
- Linux
- Fedora
categories: 精华转载
---

1、audacious:
yum install Audacious 
yum install audacious-plugins-* 

2、RPM Fusion

因为专利许可证的原因，Fedora软件仓库不包含MP3,DVD和视频播放及解码库。正因为如此，你必须从第三方的软件仓库安装那些软件，请不要担心，这是非常容易的 :)

现在我们开始安装 RPM Fusion 软件仓库，RPM Fusion 是 Fedora 和 Red Hat企业版的软件仓库，是由Dribble, Freshrpms 和 RPM Fusion合并而来的。各种各样的应用程序包含在这个软件仓库中，比如MP3、未加密的 DVD 、Mplayer, VLX, Xine 等多媒体应用程序使用的解码库，以及闭源的 Nvidia 和 ATI 显卡驱动，RPM Fusion包含以下两个主要的软件仓库：
一个被命名为“免费”，为开源软件提供（开源软件的含义通过Fedora授权指引定义），但因为美国专利保护法案又不能包含在 Fedora 中。
另一个被命名为“非免费”，为非自由软件提供，就是其它所有那些不能被免费提供的，包括公开源代码的软件，但是有“非商业使用”之类的限 制。
<!--more-->
在这部分指南的最后，我保证你安装并启用了 RPM Fusion 软件仓库，所以，打开一个终端吧，输入：

su -
rpm -ivh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm
rpm -ivh http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-stable.noarch.rpm
yum update     


输入上面的指令之后，只要选择一个mp3格式文件的双击播放，就能成功的找到mp3插件，选择安装，就好了，然后选择一个wma格式的文件双击 ，就会自动的去下载wma的插件。
