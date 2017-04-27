---
title: "git branch"
toc: true
date: 2015-07-21 12:00:00
tags:
- git
categories: 设计开发
---
# 前言
Branches in Git are incredibly lightweight as well. They are simply pointers to a specific commit -- nothing more. This is why many Git enthusiasts chant the mantra:

> branch early, and branch often.

Because there is no storage / memory overhead with making many branches, it's easier to logically divide up your work than have big beefy branches.

When we start mixing branches and commits, we will see how these two features combine. For now though, just remember that a branch essentially says "I want to include the work of this commit and all parent commits."

<!--more-->

# 创建分支
1、创建本地bugFix分支
`git branch bugFix`

2、切换到bugFix分支
```
git branch -a
git checkout bugFix
```

3、创建远程bugFix分支
`git push origin HEAD:bugFix`


# 上传分支
在bugFix分支下进行了修改，然后提交修改，命令如下：
```
git add .
git commit -m "something"
git push origin HEAD:bugFix
```

# 合并分支
## git merge
1、修改或添加文件，提交一次
`git add .`，`git commit`

2、切换回master
`git checkout master`

3、再次修改或添加文件，提交一次
`git add .`，`git commit`

4、合并bugFix分支到master
`git merge bugFix`
如果有冲突，会有提示：
```
Auto-merging test.txt
CONFLICT (content): Merge conflict in test.txt
Automatic merge failed; fix conflicts and then commit the result.
```

5、打开test.txt，可以看到类似如下冲突：
```
<<<<<<< HEAD
hello merge master
=======
hello merge bugFix
>>>>>>> bugFix
```
可以看到 ======= 隔开的上半部分，是 HEAD（即 master 分支，在运行 merge 命令时检出的分支）中的内容，下半部分是在bugFix分支中的内容。解决冲突的办法无非是二者选其一或者由你亲自整合到一起。

6、修改test.txt如下：
```
hello merge master
```

7、在解决了所有文件里的所有冲突后，运行`git add`将把它们标记为已解决（resolved）。
然后使用`git commit`命令进行提交，merge就算完成了。

8、切换到bugFix分支
`git checkout bugFix`

9、合并master分支到bugFix
`git merge master`

## git rebase
假设当前分支是bugFix，`git rebase master`进行的操作，把bugFix分支起点变成master分支的子节点。整个流程大致如下：
```
git branch bugFix
git checkout bugFix
git commit
git checkout master
git commit
git checkout bugFix
git rebase master
```


# 书签
Learn Git Branching
http://learngitbranching.js.org/

git merge简介
http://blog.csdn.net/hudashi/article/details/7664382

Git Book 中文版 - rebase
http://gitbook.liuhui998.com/4_2.html
