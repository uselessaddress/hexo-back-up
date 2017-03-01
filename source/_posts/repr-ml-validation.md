---
title: 验证迷你项目
toc: true
date: 2017-02-28 17:00:00
tags:
- 转载
- 机器学习
categories: 精华转载
---
# 项目简介
在此迷你项目中，你将从分割训练数据和测试数据开始。这是你通往“构建 POI 识别符”这个最终项目的第一步。

<!--more-->

# 第一个（过拟合）POI 识别符
你将先开始构建想象得到的最简单（未经过验证的）POI 识别符。 本节课的初始代码 (validation/validate_poi.py) 相当直白——它的作用就是读入数据，并将数据格式化为标签和特征的列表。 创建决策树分类器（仅使用默认参数），在所有数据（你将在下一部分中修复这个问题！）上训练它，并打印出准确率。 这是一颗过拟合树，不要相信这个数字！尽管如此，准确率是多少？
答：

从 Python 3.3 开始，字典键被处理的顺序发生了变化，顺序在每次代码运行时都会得到随机化处理。 这会造成与评分工具和项目代码（均在 Python 2.7 下运行）的一些兼容性问题。 要更正这个问题，向 validate_poi.py 第 25 行调用的 featureFormat 添加以下参数：
```
sort_keys = '../tools/python2_lesson13_keys.pkl'
```
这将以 Python 2 的键顺序打开 tools 文件夹中的文件。

注意：如果你没有获得评分工具期望的结果，你可能会想查看 tools/feature_format.py 文件。 由于最终项目发生的变化，一些文件更改影响了此处所写任务的数量输出。 检查你是否从资源库获得了最新版本的文件，以便 featureFormat 具有 sort_keys = False 的默认参数，并且 keys = dictionary.keys() 能够产生结果。

# 部署训练/测试机制
现在，你将添加训练和测试，以便获得一个可靠的准确率数字。 使用 sklearn.cross_validation 中的 train_test_split 验证； 将 30% 的数据用于测试，并设置 random_state 参数为 42（random_state 控制哪些点进入训练集，哪些点用于测试；将其设置为 42 意味着我们确切地知道哪些事件在哪个集中； 并且可以检查你得到的结果）。更新后的准确率是多少？
答：0.724137931034

# 源码分享
https://github.com/voidking/ud120-projects

# 书签
机器学习入门
https://cn.udacity.com/course/intro-to-machine-learning--ud120

