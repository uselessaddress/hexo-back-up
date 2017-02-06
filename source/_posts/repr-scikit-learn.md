---
title: scikit-learn
toc: true
date: 2017-02-05 10:00:00
tags:
- 转载
- 机器学习
- python
categories: 精华转载
---
# 前言
本文转载自[优达学城《机器学习工程师》](https://cn.udacity.com/course/machine-learning-engineer-nanodegree--nd009)

<!--more-->

# scikit-learn 的安装
检查您是否装有可用的 python。优达学城使用 python 2.7 作为示例代码和在浏览器中完成作业的代码。

我们会使用 pip 来安装一些程序包。首先，在[此处](https://pip.pypa.io/en/latest/installing.html)获取并安装 pip。如果你使用Anaconda, 你可以用 conda 命令来安装包。

使用 pip 或 anaconda 来安装 scikit-learn：

- 打开 Terminal（mac 下是 Terminal， PC 是 cmd）
用下列命令安装 sklearn
- `pip install scikit-learn` 或者 `conda install scikit-learn`
- 如果你不用 pip 或者 conda，可以在[这里](http://scikit-learn.org/stable/install.html)找到安装说明。

# 关于 scikit-learn 版本的重要通知
scikit-learn 最近把稳定版升级到了 v0.18。这次升级改变了一些我们将要在课程中讲到的函数的调用方法，例如：train_test_split、gridSearchCV、ShuffleSplit 和 learning_curves。scikit-learn 网站上的文档已经更新到了 v0.18。但是 Katie 导师的讲解以及优达学城（Udacity）的练习，作业还是基于v0.17。如果你需要查询 scikit-learn 的文档，请查询 [v0.17 的说明](http://scikit-learn.org/0.17/)，而非 v0.18。近期我们会把内容统一升级成 v0.18。

[这个论坛链接](http://discussions.youdaxue.com/t/sklearn-0-18/8282)提供了更加详细的说明。如果你还有疑问，可以在论坛和微信群里提出。

# Scikit-learn 代码
在接下来的部分中，Katie 会演示如何将 scikit-learn（或 sklearn）文档与在“机器学习简介”课程中介绍的高斯朴素贝叶斯模型一起使用。对于本练习，您不必熟悉朴素贝叶斯或者 Katie 演示的代码，而是要熟悉 sklearn 的布局，以便之后能评估和验证任何数据模型。

在即将开始的“监督式机器学习”课程中，我们会更详细地介绍朴素贝叶斯以及其他有用的受监督模型，并运用我们在本课程中学到的知识评估每个模型的优缺点。

如果想提前了解一下朴素贝叶斯，请查看此[链接](http://scikit-learn.org/stable/modules/naive_bayes.html)。

# sklearn使用入门
在谷歌上搜索“sklearn naive bayes”即可。

# 高斯朴素贝叶斯示例
http://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.GaussianNB.html

# 有关地形数据的高斯 NB 部署

## studentMain.py
```
# -*- coding: UTF-8 -*-
#!/usr/bin/python

""" Complete the code in ClassifyNB.py with the sklearn
    Naive Bayes classifier to classify the terrain data.
    
    The objective of this exercise is to recreate the decision 
    boundary found in the lesson video, and make a plot that
    visually shows the decision boundary """


from prep_terrain_data import makeTerrainData
from class_vis import prettyPicture, output_image
from ClassifyNB import classify

import numpy as np
import pylab as pl


features_train, labels_train, features_test, labels_test = makeTerrainData()

### the training data (features_train, labels_train) have both "fast" and "slow" points mixed
### in together--separate them so we can give them different colors in the scatterplot,
### and visually identify them
grade_fast = [features_train[ii][0] for ii in range(0, len(features_train)) if labels_train[ii]==0]
bumpy_fast = [features_train[ii][1] for ii in range(0, len(features_train)) if labels_train[ii]==0]
grade_slow = [features_train[ii][0] for ii in range(0, len(features_train)) if labels_train[ii]==1]
bumpy_slow = [features_train[ii][1] for ii in range(0, len(features_train)) if labels_train[ii]==1]


# You will need to complete this function imported from the ClassifyNB script.
# Be sure to change to that code tab to complete this quiz.
clf = classify(features_train, labels_train)



### draw the decision boundary with the text points overlaid
prettyPicture(clf, features_test, labels_test)
output_image("test.png", "png", open("test.png", "rb").read())


```

## class_vis.py
```
# -*- coding: UTF-8 -*-
#!/usr/bin/python

#from udacityplots import *
import warnings
warnings.filterwarnings("ignore")

import matplotlib 
matplotlib.use('agg')

import matplotlib.pyplot as plt
import pylab as pl
import numpy as np

#import numpy as np
#import matplotlib.pyplot as plt
#plt.ioff()

def prettyPicture(clf, X_test, y_test):
    x_min = 0.0; x_max = 1.0
    y_min = 0.0; y_max = 1.0

    # Plot the decision boundary. For that, we will assign a color to each
    # point in the mesh [x_min, m_max]x[y_min, y_max].
    h = .01  # step size in the mesh
    xx, yy = np.meshgrid(np.arange(x_min, x_max, h), np.arange(y_min, y_max, h))
    Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])

    # Put the result into a color plot
    Z = Z.reshape(xx.shape)
    plt.xlim(xx.min(), xx.max())
    plt.ylim(yy.min(), yy.max())

    plt.pcolormesh(xx, yy, Z, cmap=pl.cm.seismic)

    # Plot also the test points
    grade_sig = [X_test[ii][0] for ii in range(0, len(X_test)) if y_test[ii]==0]
    bumpy_sig = [X_test[ii][1] for ii in range(0, len(X_test)) if y_test[ii]==0]
    grade_bkg = [X_test[ii][0] for ii in range(0, len(X_test)) if y_test[ii]==1]
    bumpy_bkg = [X_test[ii][1] for ii in range(0, len(X_test)) if y_test[ii]==1]

    plt.scatter(grade_sig, bumpy_sig, color = "b", label="fast")
    plt.scatter(grade_bkg, bumpy_bkg, color = "r", label="slow")
    plt.legend()
    plt.xlabel("bumpiness")
    plt.ylabel("grade")

    plt.savefig("test.png")
    
import base64
import json
import subprocess

def output_image(name, format, bytes):
    image_start = "BEGIN_IMAGE_f9825uweof8jw9fj4r8"
    image_end = "END_IMAGE_0238jfw08fjsiufhw8frs"
    data = {}
    data['name'] = name
    data['format'] = format
    data['bytes'] = base64.encodestring(bytes)
    print image_start+json.dumps(data)+image_end
```

## prep_terrain_data.py
```
# -*- coding: UTF-8 -*-
#!/usr/bin/python
import random


def makeTerrainData(n_points=1000):
###############################################################################
### make the toy dataset
    random.seed(42)
    grade = [random.random() for ii in range(0,n_points)]
    bumpy = [random.random() for ii in range(0,n_points)]
    error = [random.random() for ii in range(0,n_points)]
    y = [round(grade[ii]*bumpy[ii]+0.3+0.1*error[ii]) for ii in range(0,n_points)]
    for ii in range(0, len(y)):
        if grade[ii]>0.8 or bumpy[ii]>0.8:
            y[ii] = 1.0

### split into train/test sets
    X = [[gg, ss] for gg, ss in zip(grade, bumpy)]
    split = int(0.75*n_points)
    X_train = X[0:split]
    X_test  = X[split:]
    y_train = y[0:split]
    y_test  = y[split:]

    grade_sig = [X_train[ii][0] for ii in range(0, len(X_train)) if y_train[ii]==0]
    bumpy_sig = [X_train[ii][1] for ii in range(0, len(X_train)) if y_train[ii]==0]
    grade_bkg = [X_train[ii][0] for ii in range(0, len(X_train)) if y_train[ii]==1]
    bumpy_bkg = [X_train[ii][1] for ii in range(0, len(X_train)) if y_train[ii]==1]

#    training_data = {"fast":{"grade":grade_sig, "bumpiness":bumpy_sig}
#            , "slow":{"grade":grade_bkg, "bumpiness":bumpy_bkg}}


    grade_sig = [X_test[ii][0] for ii in range(0, len(X_test)) if y_test[ii]==0]
    bumpy_sig = [X_test[ii][1] for ii in range(0, len(X_test)) if y_test[ii]==0]
    grade_bkg = [X_test[ii][0] for ii in range(0, len(X_test)) if y_test[ii]==1]
    bumpy_bkg = [X_test[ii][1] for ii in range(0, len(X_test)) if y_test[ii]==1]

    test_data = {"fast":{"grade":grade_sig, "bumpiness":bumpy_sig}
            , "slow":{"grade":grade_bkg, "bumpiness":bumpy_bkg}}

    return X_train, y_train, X_test, y_test
#    return training_data, test_data
```

## ClassifyNB.py
```
# -*- coding: UTF-8 -*-
def classify(features_train, labels_train):   
    ### import the sklearn module for GaussianNB
    ### create classifier
    ### fit the classifier on the training features and labels
    ### return the fit classifier
    
    
    ### your code goes here!
    def classify(traindata,trainlabel):
        from sklearn.naive_bayes import GaussianNB
        classifier=GaussianNB()
        classifier.fit(traindata, trainlabel)
        return classifier
```

