---
title: 决策树迷你项目
toc: true
date: 2017-02-18 10:00:00
tags:
- 转载
- 机器学习
categories: 精华转载
---
# 项目背景
在本项目中，我们将再次尝试确认邮件作者，但这次使用的是决策树。初始代码可以在 decision_tree/dt_author_id.py 中找到。

你仍需要在你计算机上完成迷你项目，在浏览器中输入答案。你可以[在这里](https://s3.cn-north-1.amazonaws.com.cn/static-documents/nd002/DTMini-Project_zh.pdf)找到决策树迷你项目的说明。

<!--more-->

# 运行起来
使用 decision_tree/dt_author_id.py 中的初始代码，准备好决策树并将它作为分类器运行起来，设置 min_samples_split=40。可能需要等一段时间才能开始训练。

准确率是多少？

# 加速
你从 SVM 迷你项目中了解到，参数调整可以显著加快机器学习算法的训练时间。一般情况下，参数可以调整算法的复杂度，越复杂的算法通常运行起来越慢。

控制算法复杂度的另一种方法是通过你在训练/测试时用到的特征数量。算法可用的特征数越多，越有可能发生复杂拟合。我们将在“特征选择”这节课中详细探讨，但你现在可以提前有所了解。

你数据中的特征数是多少？

（提示：数据被整理成一个 numpy 数组后，行数是数据点数，列数是特征数；要提取这个数字，只需运行代码 len(features_train[0])。）

# 源码分享
https://github.com/voidking/ud120-projects

# 书签
机器学习入门
https://cn.udacity.com/course/intro-to-machine-learning--ud120

