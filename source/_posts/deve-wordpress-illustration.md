title: WordPress使用说明
date: 2013-11-05 13:25:26
tags:
- WordPress
- PHP
- mysql
categories: 设计开发
---

## WordPress使用说明

### 前言
WordPress是一个免费的开源项目，在GNU通用公共许可证下授权发布。是一个注重美学、易用性和网络标准的个人信息发布平台。
简体中文官网：http://cn.wordpress.org

当年，刚开始接触建站，和WordPress留下了一段难忘的时光。

### 本地环境
由于WordPresss是使用PHP语言和MySQL数据库开发的。所以，本地搭建测试环境，需要PHP和MySQL。
<!--more-->
这里推荐两个套件：
1、XAMPP
https://www.apachefriends.org/index.html

2、ServKit（曾经叫做PHPnow）
http://servkit.org/


### 本地问题解决
1、Attempting to start Tomcat service...
双击tomcat7.exe，提示不是有效的32位程序。但是在命令行敲startup可以正常启动。自我感觉最好的解决办法是把可以正常使用的tomcat文件夹替换掉xampp的tomcat文件夹。

2、安装xampp后apache不能启动解决方法
http://blog.csdn.net/kunlong0909/article/details/7716715

3、MySQL Service detected with wrong path
首先需要修改一下环境变量。
应该是在安装xampp之前电脑上装过mysql，然后默认启动的是以前的mysql。
修改注册表[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MySQL]的ImagePath修改成新的xampp中位置<xampp>\mysql\bin\mysqld MySQL
重启explorer.exe进程，使注册表生效
再次点击 mysql 后边的start，OK！mysql服务正常启动！ 

4、安装wordpress过程中，访问wp-admin/install.php出错：数据库连接错误。您在wp-config.php文件中提供的数据库用户名和密码可能不正确
[crifan的博客](http://www.crifan.com/during_install_wordpress_access_wp_admin_install_php_error_database_connection_error_your_database_name_and_password_may_not_correct_provided_in_wp_config_php/)

### 优化
1、在wordpress功能版块有登入\登出，管理，Feed以外，还有一个wordpress，如何删除它们呢，看起来使博客的所有链接实际有用！
找到网站wordpress安装目录，在wp_includes文件夹下面有一个default-widgets.php的文件，在此文件里搜索查找<li>标签里带有rss2_url，comments_rss2_url，wordpress.org的三段代码全部删除掉。

2、WordPress文章置顶
1、写好文章并发布。
2、点击博客后台文章菜单下的“编辑”选项，进入文章列表。
3、把鼠标移到需要置顶的文章上，点击“快速编辑”选项。
4、在快速编辑下“保持这篇文章置顶”前面的小框打勾，然后点击更新文章。
5、更新文章后，打开博客首页就会发现文章的置顶状态了。
Tips：除了使用这个方法，也可以使用WordPree的一些文章置顶插件，相对来说可能会方便许多，在这里不做推荐。

3、WordPress评论框下方“您可以使用这些 HTML 标签和属性”文字去除
找到wp-includes/comment-template.php  
查找：
echo $args['comment_notes_after'];
注释掉它（也可以直接删除）。


### 参考文档

PHP 手册
http://php.net/manual/zh/

------
WordPress中文文档 | WordPress啦
http://www.wordpress.la/codex.html

------
WordPress官方中文文档
http://codex.wordpress.org/zh-cn:Main_Page

------
WordPress连接微博:个人博客开放化策略微博QQ百度账号登录
http://www.freehao123.com/wordpress-weibo-qq/

------
怎样在wordpress网站添加小工具
http://www.zhidao91.com/wordpress-add-widget-area/

------
如何轻松的在你的wordpress网站添加附加小工具或者导航区域
http://www.ylsay029.com/how-to-add-widget-or-navigation-areas-wordpress-website.html
http://www.douban.com/note/285551052/

------
新版京东云擎JAE云空间申请使用和安装运行WordPress博客
http://www.freehao123.com/jae-wordpress/

------
增强Wordpress编辑器功能最简便的方法
http://jingyan.baidu.com/article/7c6fb42841891b80652c906b.html

------
两步搞定WordPress多区域widget
http://www.wordpress.la/widgetize-wordpress-theme.html
http://www.veryhuo.com/a/view/15116.html

------
WordPress 博客添加个性标志favicon.ico
http://www.chinaz.com/web/2010/0928/135480.shtml

------
中文版标签云插件：wp-cumulus
http://plugins.wopus.org/content/tagplugin/146.html

------
29个实用的WordPress主题函数使用技巧.pdf、WordPress主题教程.pdf、WordPress_主题模板制作及修改教程.pdf
http://yunpan.cn/cVbAQzcvut7FS  访问密码 dd25

------
wordpress代码调用大全更新到_3.0版.pdf、WordPress高级教程.pdf、如何用WORDPRESS改成CMS来开发企业站.pdf
http://yunpan.cn/cVbAqs5Bd32pS  访问密码 82bc

------