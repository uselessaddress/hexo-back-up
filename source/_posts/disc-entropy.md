---
title: 信息量&信息熵&信息增益
toc: true
date: 2017-03-12 20:00:00
tags:
- 机器学习
categories: 点滴发现
---
# 信息量
确定性是指事先可以准确知道某个事件或某种决策的结果。或者说，如果事件或决策的可能结果只有一种，就会产生确定性。

信息论创始人香农对信息的定义：信息是确定性的增加。

> 信息量是信息多少的度量，也就是确定性增加的多少的度量，单位是比特。

如何计算信息量的多少？在日常生活中，极少发生的事件一旦发生是容易引起人们关注的，而司空见惯的事不会引起注意，也就是说，极少见的事件所带来的信息量多。如果用统计学的术语来描述，就是出现概率小的事件信息量多。因此，事件出现得概率越小，信息量愈大。即信息量的多少是与事件发生频繁（即概率大小）成反比。

<!--more-->

如已知事件$x_i$已发生，则表示$x_i$所含有或所提供的信息量
$$
I(x_i) = -\log 2^{p(x_i)}
$$

例题：若估计在一次国际象棋比赛中谢军获得冠军的可能性为0.1（记为事件A），而在另一次国际象棋比赛中她得到冠军的可能性为0.9（记为事件B）。试分别计算当你得知她获得冠军时，从这两个事件中获得的信息量各为多少？
$$
I(A) = -\log 2^{p(0.1)} ≈ 3.32（比特）
$$

$$
I(B) = -\log 2^{p(0.9)} ≈ 0.152（比特）
$$


# 信息熵
不确定性是指事先不能准确知道某个事件或某种决策的结果。或者说，只要事件或决策的可能结果不止一种，就会产生不确定性。

> 信息熵就是用以消除事件的不确定性所需要的信息量，单位是比特。

根据小编的理解，给出两个对比描述：
1、这个杯子的容量是2L，容量是指物体或者空间所能够容纳的单位物体的数量。
2、这个事件的信息熵是3bit，信息熵是指消除事件的不确定性所需要的单位信息的数量。
也就是说，就像容量是用来描述空间大小的度量一样，信息熵就是用来描述不确定性大小的度量。

通常我们使用H(X)表示随机变量（事件）的熵：
$$
H(X) = E(I(X))
$$
其中的 E 代表期望值函數， I(X) 代表信息量所形成的随机变量（变量X的函数结果表示I(X)也是变量）。

$$
H(X) = E(I(X)) = \sum_{i=1}^n {p(x_i)\,I(x_i)} = -\sum_{i=1}^n {p(x_i) \log 2^{p(x_i)}} 
$$

其中$x_i$表示第i个状态（总共有n种状态），$p(x_i)$表示第i个状态出现的概率。

> 条件熵就是在事件X确定的条件下，用以消除事件Y的不确定性所需要的信息量。

以下推导，log函数默认以2为底数。

\begin{eqnarray} 
H(Y|X) &=& \sum_{x\in\mathcal X}\,p(x)\,H(Y|X=x)\\ 
&=& -\sum_{x\in\mathcal X}p(x)\sum_{y\in\mathcal Y}\,p(y|x)\,\log\,p(y|x)\\ 
&=& -\sum_{x\in\mathcal X}\sum_{y\in\mathcal Y}\,p(y,x)\,\log\,p(y|x)\\ 
&=& -\sum_{x\in\mathcal X, y\in\mathcal Y}p(x,y)\log\,p(y|x). 
\end{eqnarray}

条件熵 H(Y|X) 相当于联合熵 H(Y,X) 减去单独的熵 H(X)，其数学式如下。

\begin{align} 
H(Y|X) = H(Y,X)-H(X)   
\end{align}

> 联合熵就是用以消除多个同时发生的事件的不确定性所需要的信息量。

以下证明联合熵和条件熵的关系。
\begin{eqnarray} 
H(X,Y) 
&=& -\sum_{x\in\mathcal X, y\in\mathcal Y}p(x,y)log\,p(x,y)\\ 
&=& -\sum_{x\in\mathcal X, y\in\mathcal Y}p(x,y)log\left(p(y|x)p(x)\right)\\ 
&=& -\sum_{x\in\mathcal X, y\in\mathcal Y}p(x,y)log\,p(y|x) - \sum_{x\in\mathcal X, y\in\mathcal Y} p(x,y) log\,p(x)\\ 
&=& H(Y|X)-\sum_{x\in\mathcal X, y\in\mathcal Y}p(x,y)log\,p(x)\\ 
&=& H(Y|X)-\sum_{x\in\mathcal X}\sum_{y\in\mathcal Y}p(x,y)log\,p(x)\\ &=& H(Y|X)-\sum_{x\in\mathcal X}log\,p(x)\sum_{y\in\mathcal Y}p(x,y)\\ &=& H(Y|X)-\sum_{x\in\mathcal X}(log\,p(x))p(x)\\ 
&=& H(Y|X)-\sum_{x\in\mathcal X}p(x)log\,p(x)\\ 
&=& H(Y|X)+H(X)\\ &=& H(X)+H(Y|X)\\
\end{eqnarray}

# 信息增益
特征选择是指从全部特征中选择一个特征子集，使构造出来的模型更好。

在机器学习的实际应用中，特征数量往往较多，其中可能存在不相关的特征，特征之间也可能存在相互依赖，容易导致训练时间过长、维度灾难等。
特征选择能剔除不相关或冗余的特征，从而达到减少特征个数，提高模型精确度，减少运行时间的目的。另一方面，选取出真正相关的特征，简化了模型。

在训练样本集上，运行C4.5或其他决策树生成算法，待决策树充分生长后，再在树上运行剪枝算法，则最终决策树各分支处的特征就是选出来的特征子集。决策树方法一般使用信息增益作为评价函数。

在信息增益中，衡量标准是看特征能够为分类系统带来多少信息，带来的信息越多，该特征就越重要。对一个特征而言，系统有它和没它时信息量将发生变化，而前后信息量的差值就是这个特征给系统带来的信息量。

> 信息增益定义为父项熵减去分割父项后生成的子项的熵的加权平均。

特征T给聚类C或分类C带来的信息增益为：
$$
IG(T) = H(C) - H(C|T)
$$

# 书签
信息量
http://baike.baidu.com/item/%E4%BF%A1%E6%81%AF%E9%87%8F

信息熵
http://baike.baidu.com/item/%E4%BF%A1%E6%81%AF%E7%86%B5

信息增益
http://baike.baidu.com/item/%E4%BF%A1%E6%81%AF%E5%A2%9E%E7%9B%8A

互資訊與條件熵
http://ccckmit.wikidot.com/st:mutualinformation

c4.5为什么使用信息增益比来选择特征？
https://www.zhihu.com/question/22928442/answer/117189907

信息增益到底怎么理解呢？
https://www.zhihu.com/question/22104055/answer/67014456

[优达学城-机器学习入门-熵和信息增益](https://classroom.udacity.com/courses/ud120/lessons/2258728540/concepts/24488285540923)





