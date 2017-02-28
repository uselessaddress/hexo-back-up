---
title: 主成分分析迷你项目
toc: true
date: 2017-02-28 12:00:00
tags:
- 转载
- 机器学习
categories: 精华转载
---
# 项目简介
我们在讨论 PCA 时花费了大量时间来探讨理论问题，因此，在此迷你项目中，我们将要求你写一些 sklearn 代码。特征脸方法代码很有趣，而且内容丰富，足以胜任这一整个迷你项目的试验平台。

可在 [pca/eigenfaces.py](https://github.com/udacity/ud120-projects/blob/master/pca/eigenfaces.py) 中找到初始代码。此代码主要取自[此处](http://scikit-learn.org/stable/auto_examples/applications/face_recognition.html) sklearn 文档中的示例。

请注意，在运行代码时，对于在 pca/eigenfaces.py 的第 94 行调用的 SVC 函数，有一个参数有改变。对于“class_weight”参数，参数字符串“auto”对于 sklearn 版本 0.16 和更早版本是有效值，但将被 0.19 舍弃。如果运行 sklearn 版本 0.17 或更高版本，预期的参数字符串应为“balanced”。如果在运行 pca/eigenfaces.py 时收到错误或警告，请确保第 98 行包含与你安装的 sklearn 版本匹配的正确参数。

如果直接运行下载的代码，会先下载233MB的数据文件。你可以点击[这里](http://cn-static.udacity.com/mlnd/eigenfaces.zip)先下载数据集，再根据指示运行代码。

<!--more-->

# 主成分的可释方差
我们提到 PCA 会对主成分进行排序，第一个主成分具有最大方差，第二个主成分 具有第二大方差，依此类推。第一个主成分可以解释多少方差？第二个呢？

我们发现，有时 Pillow 模块（本例中使用的）可能会造成麻烦。如果你收到与 fetch_lfw_people() 命令相关的错误，请尝试以下命令：
`pip install --upgrade PILLOW`

如果运行时遇到错误，请注意对于在“pca/eigenfaces.py ”的第 94 行调用的“SVC”函数，有一个参数有改变。对于“class_weight”参数，参数字符串“auto”对于 sklearn 版本 0.16 和更早版本是有效值，但将被 0.19 版本舍弃。如果运行 sklearn 版本 0.17 或更高版本，预期的参数字符串应为“balanced”。如果在运行“pca/eigenfaces.py”时收到错误或警告，请确保第 98 行包含与你安装的 sklearn 版本匹配的正确参数。

# 要使用多少个主成分？
现在你将尝试保留不同数量的主成分。在类似这样的多类分类问题中（要应用两个以上标签），准确性这个指标不像在两个类的情形中那么直观。相反，更常用的指标是 F1 分数。

我们将在评估指标课程中学习 F1 分数，但你自己要弄清楚好的分类器的特点是具有高 F1 分数还是低 F1 分数。你将通过改变主成分数量并观察 F1 分数如何相应地变化来确定。

随着你添加越来越多的主成分作为训练分类器的特征，你认为它的性能会更好还是更差？
答：更好

# F1 分数与使用的主成分数
将 n_components 更改为以下值：[10, 15, 25, 50, 100, 250]。对于每个主成分，请注意 Ariel Sharon 的 F1 分数。（对于 10 个主成分，代码中的绘制功能将会失效，但你应该能够看到 F1 分数。）

如果看到较高的 F1 分数，这意味着分类器的表现是更好还是更差？
答：更好

# 维度降低与过拟合
在使用大量主成分时，是否看到过拟合的证据？
答：是的，当使用大量主成分时，性能开始下降。

# 源码分享
https://github.com/voidking/ud120-projects

# 书签
机器学习入门
https://cn.udacity.com/course/intro-to-machine-learning--ud120

