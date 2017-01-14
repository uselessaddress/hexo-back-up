title: node依赖包的管理
toc: true
date: 2016-05-10 22:50:26
tags: 
- node
- npm
- git
- bower
categories: 设计开发
---
# 前言
在使用node进行开发时，我们会用到很多包（模块、中间件），而这些包，并不是需要我们维护的，所以，只要记住包名和版本号就够了。而这些包名和版本号等信息，就放在package.json文件里，该文件就类似于Maven中的pom.xml。

package.json文件，可以手动编写，也可以利用`npm init`命令生成。

<!--more-->

# npm常用命令

1、`npm install module-name -g`，全局安装。
2、`npm install module-name --save`，自动把模块和版本号添加到dependencies部分。
3、`spm install module-name --save-dev` 自动把模块和版本号添加到devDependencies部分。
那么，2和3有什么区别呢？根据官方的解释，就是说在dependencies部分用到的模块，是一些必须的模块；而devDependencies用到的模块，是我们在生产过程中做测试用到的模块。如果你把自己的代码提交到了git服务器，别人下载了你的代码，然后`npm install`，下载所有依赖的模块，这时，dependencies里的模块会下载，devDependencies里的模块不会下载。

# 忘记使用--save补救
`npm init`，根据提示可以创建一个package.json文件，这个过程会自动把已经安装的模块写入package.json的dependencies部分。

假设，在之后，我们又安装了很多模块，但是，在安装时，使用的命令是`npm install`，而不是`npm install --save`。这时，有没有什么办法，可以一次性把我们安装的所有模块名和版本号写入package.json呢？

小编摸索出来一个可行的办法：
1、把原先的package.json（文件1）拷贝出来。
2、`npm init`，不停回车至新的package.json（文件2）创建完成。
3、把文件2的dependencies部分拷贝覆盖到文件1的dependencies部分。
4、使用文件1覆盖文件2。
5、`npm ls`，如果有报错，说明还有个别依赖模块没有写进package.json。
6、根据5的提示，手动修改package.json；或者，`npm install module-name --save`，把没有写进package.json的模块重新安装一次。

# git忽略上传
在主目录下新建.gitignore文件，内容格式如下：
1、最常用
```
/node_modules/*
/bower_components/*
```

2、在已忽略文件夹中不忽略指定文件夹
```
/node_modules/*
!/node_modules/layer/
```

3、在已忽略文件夹中不忽略指定文件
```
/node_modules/*
!/node_modules/layer/layer.js
```
【注意项】注意写法 要忽略的文件夹一定要结尾 `/*` ，否则不忽略规则将无法生效

4、其他规则写法 (附)

- 以斜杠“/”开头表示目录；

- 以星号“*”通配多个字符；

- 以问号“?”通配单个字符

- 以方括号“[]”包含单个字符的匹配列表；

- 以叹号“!”表示不忽略(跟踪)匹配到的文件或目录；


# 后记

bower的用法和npm非常相似，两者可以对比着学习和记忆。

# 参考文档
npm Documentation
https://docs.npmjs.com/install

package.json
https://docs.npmjs.com/files/package.json

git添加 .ignore 忽略
http://www.thinksaas.cn/topics/0/487/487787.html  

.gitignore 规则写法   
http://my.oschina.net/longyuan/blog/521098

[用package.json来管理NPM里面的依赖包](http://www.shuizhongyueming.com/2014/07/12/use-package-json-to-manage-the-dependencies-package-of-npm/)

淘宝npm镜像
http://npm.taobao.org/
