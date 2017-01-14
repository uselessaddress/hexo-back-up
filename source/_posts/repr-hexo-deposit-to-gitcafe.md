title: hexo托管到gitcafe
date: 2015-05-31 11:28:36
tags: 
- hexo
- gitcafe
- 网站
categories: 设计开发
---

### 前言
gitcafe作为国内的代码托管网站，访问速度远快于github，聪明的同学（比如人家我），肯定都会选择gitcafe吧！
在《Hexo环境搭建》中，我们已经讲过了SSH key的生成和使用方法。下面还要用到SSH key，没学会小伙伴的自行百度。

### 注册
官网：http://www.gitcafe.com

### 创建Project
https://gitcafe.com/projects/new ，创建和用户名相同的project，比如我的用户名为voidking，那么project名称就为voidking。


### 添加SSH key
https://gitcafe.com/account/public_keys ，Add a new public key，复制id_rsa.pub中的内容，粘贴进去即可。

### 测试连接
`ssh git@gitcafe.com`

```
Hi voidking! You've successfully authenticated, but GitCafe does not provide shell access.
Connection to gitcafe.com closed.
```

### 小结
通过连接github和gitcafe，我们可以得出结论：
在本地生成密钥的时候，根本不用考虑今后的服务器。github还是gitcafe，或者任何其他使用git的服务器，都没有关系。在需要使用某个服务器时，只需要把密钥添加到该服务器上面，就完成了配置工作。


### 修改_config.yml
编辑E:\hexo下的_config.yml，修改Deployment部分：

```
deploy:
  type: git
  repository: git@gitcafe.com:voidking/voidking.git
  branch: gitcafe-pages
```
<!--more-->
### 部署
`deploy d`，不需要输入密码，直接就可以上传到gitcafe。

### 访问测试
访问：http://voidking.gitcafe.io
或者：http://voidking.com
或者：http://www.voidking.com


### 参考文档
托管博客到gitcafe
http://zipperary.com/2013/11/23/hexo-to-gitcafe/
