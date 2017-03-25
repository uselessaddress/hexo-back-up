--- 
title: gitignore用法
toc: true
date: 2015-07-21 10:00:00
tags:
- git
- gitignore
categories: 设计开发
---
# 前言
有些时候，我们必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件、项目运行时生成的临时文件等等，每次`git status`都会显示`Untracked files ...`，让人不爽。

好在Git考虑到了大家的感受，在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

<!--more-->

# gitignore规则
## 基本语法
- 以斜杠`/`开头表示目录
- 以星号`*`通配多个字符；
- 以问号`?`通配单个字符
- 以方括号`[]`包含单个字符的匹配列表
- 以叹号`!`表示不忽略(跟踪)匹配到的文件或目录

## 示例
```
# 这是注释行，将被忽略
*.a       # 忽略所有以.a为扩展名的文件    
!lib.a    # 但是名为lib.a的文件或目录不要忽略，即使前面设置了对*.a的忽略
/TODO     # 只忽略此目录下的TODO文件，子目录中的TODO文件不忽略
build/    # 忽略所有build目录下的文件，但如果是名为build的文件则不忽略
doc/*.txt # 忽略文件如doc/notes.txt，但是文件如doc/server/arch.txt不忽略
```


# push之后添加gitignore
gitignore只能作用于 Untracked Files，如果某些文件（add和commit过的文件）已经被纳入了版本管理中，则修改gitignore是无效的。解决方法就是先把本地缓存删除（改变成Untracked状态），然后再提交。

```
git pull
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
git push
```


# 书签
Github使用.gitignore文件忽略不必要上传的文件
http://blog.csdn.net/gjy211/article/details/51607347

Git忽略规则
http://www.cnblogs.com/qwertWZ/archive/2013/03/26/2982231.html

[忽略特殊文件](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013758404317281e54b6f5375640abbb11e67be4cd49e0000)

A collection of useful .gitignore templates
https://github.com/github/gitignore

解决Git在添加ignore文件之前就提交了项目无法再过滤问题
http://www.2cto.com/kf/201612/571312.html
