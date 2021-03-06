---
title: 回归迷你项目
toc: true
date: 2017-02-22 10:00:00
tags:
- 转载
- 机器学习
categories: 精华转载
---
# 项目简介
在此项目中，你将使用回归来预测安然雇员和合伙人的财务数据。一旦你知道某位雇员的财务数据，比如工资，你是否会预测他们奖金的数额？

<!--more-->

# 目标和特征
运行在 regression/finance_regression.py 中找到的初始代码。这将绘制出一个散点图，其中有所有的数据点。你尝试预测什么目标？用来预测目标的输入特征是什么？

在脑海中描绘出你大致预测的回归线（如果打印散点图并用纸笔来描绘，效果会更好）。

# 可视化回归数据
就像在分类中一样，你需要在回归中训练和测试数据。这在初始代码中已被设定。将 test_color 的值从“b”改为“r”（针对“red”），然后重新运行。

注意：对于将 Python 2 代码转换至 Python 3 的学员，请参见以下关于兼容性的重要备注。

你将仅使用蓝色（训练）点来拟合回归。（你可能已经注意到，我们放入测试集的是 50% 的数据而非标准的 10%—因为在第 5 部分中，我们将改变训练和测试数据集，并且平均分割数据使这种做法更加简单。）
从 Python 3.3 版本开始，字典的键值顺序有所改变，在每次代码运行时，字典的键值皆为随机排序。这会让我们在 Python 2.7 环境下工作的评分者遭遇一些兼容性的问题。为了避免这个问题，请在 finance_regression.py 文件的第26行 featureFormat 调用时添加一个参数

sort_keys = '../tools/python2_lesson06_keys.pkl'

它会打开 tools 文件夹中带有 Python 2 键值顺序的数据文件。

# 提取斜率和截距
从 sklearn 导入 LinearRegression 并创建/拟合回归。将其命名为 reg，这样绘图代码就能将回归覆盖在散点图上呈现出来。回归是否大致落在了你期望的地方？

提取斜率（存储在 reg.coef_ 属性中）和截距。

斜率和截距是多少？
答：斜率为5.44814029，截距为-102360.543294

# 训练数据
假设你是一名悟性不太高的机器学习者，你没有在测试集上进行测试，而是在你用来训练的相同数据上进行了测试，并且用到的方法是将回归预测值与训练数据中的目标值（比如：奖金）做对比。

你找到的分数是多少？你可能对“良好”分数还没有概念；此分数不是非常好（但却非常糟糕）。
答：0.0455091926995

# 测试数据
现在，在测试数据上计算回归的分数。

测试数据的分数是多少？如果只是错误地在训练数据上进行评估，你是否会高估或低估回归的性能？
答：-1.48499241737

# 根据 LTI 回归奖金
我们有许多可用的财务特征，就预测个人奖金而言，其中一些特征可能比余下的特征更为强大。例如，假设你对数据做出了思考，并且推测出“long_term_incentive”特征（为公司长期的健康发展做出贡献的雇员应该得到这份奖励）可能与奖金而非工资的关系更密切。

证明你的假设是正确的一种方式是根据长期激励回归奖金，然后看看回归是否显著高于根据工资回归奖金。根据长期奖励回归奖金—测试数据的分数是多少？
答：-0.59271289995

# 工资与 LTI
如果你必须预测某人的奖金并且你只有一小段相关信息，你想要知道他们的工资还是长期奖励？
答：长期奖励

# 异常值破坏回归
这是下节课的内容简介，关于异常值的识别和删除。返回至之前的一个设置，你在其中使用工资预测奖金，并且重新运行代码来回顾数据。你可能注意到，少量数据点落在了主趋势之外，即某人拿到高工资（超过 1 百万美元！）却拿到相对较少的奖金。此为异常值的一个示例，我们将在下节课中重点讲述它们。

类似的这种点可以对回归造成很大的影响：如果它落在训练集内，它可能显著影响斜率/截距。如果它落在测试集内，它可能比落在测试集外要使分数低得多。就目前情况来看，此点落在测试集内（而且最终很可能降低分数）。让我们做一些处理，看看它落在训练集内会发生什么。在 finance_regression.py 底部附近并且在 plt.xlabel(features_list[1]) 之前添加这两行代码：

reg.fit(feature_test, target_test)
plt.plot(feature_train, reg.predict(feature_train), color="b")

现在，我们将绘制两条回归线，一条在测试数据上拟合（有异常值），一条在训练数据上拟合（无异常值）。来看看现在的图形，有很大差别，对吧？单一的异常值会引起很大的差异。

新的回归线斜率是多少？
答：2.27410114

（你会发现差异很大，多数情况下由异常值引起。下一节课将详细介绍异常值，这样你就有工具来检测和处理它们了。）

# 源码分享
https://github.com/voidking/ud120-projects

# 书签
机器学习入门
https://cn.udacity.com/course/intro-to-machine-learning--ud120

