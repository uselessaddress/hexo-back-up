---
title: matlab入门
toc: true
date: 2017-01-31 09:00:00
tags:
- matlab
categories: 设计开发
---
# 前言
> MATLAB是美国MathWorks公司出品的商业数学软件，用于算法开发、数据可视化、数据分析以及数值计算的高级技术计算语言和交互式环境，主要包括MATLAB和Simulink两大部分。

读研以来，耳边不时听到“matlab”，被老师和同学普遍推崇。今天，小编就来学习一下matlab的基础操作。

<!--more-->

# 基础设置
**显示类型**
计算圆面积时，面积的结果显示，默认是short类型（保留小数点后4位），我们可以通过设置为long类型，来显示更精确的结果。
1）通过命令设置
`format long`，这种设置方式临时性的。

2）通过界面设置
File，Preferences，Command Window，Numeric format选择long，之后Apply即可。

**清屏**
清屏命令：`clc`

**清数据**
清数据命令：`clear`

**修改变量**
在workspace中，双击变量，可以在图形化界面修改变量值。

**绘图**
在workspace中，单击变量，然后点击plot。

**查看变量信息**
`who`，`whos`，`whos a`

**重新执行命令**
在command history中，找到一条命令，右键选择evaluate selection。

**保存变量**
`save a`，保存变量到当前工作路径，文件名为a.mat。

**加载数据文件**
`load a`或者`load a.mat`。

**编辑器窗口**
`edit`，打开编辑窗口，新建matlab程序。

**图像窗口**
`figure`

**GUI窗口**
`guide`

**添加搜索路径**
File，Set Path，Add Folder。

**设置初始路径**
右键matlab快捷方式，在快捷方式选项卡中，修改起始位置。

**查找函数路径**
`which sin`

# 设置默认编码
中文时，Matlab默认编码格式为GB2312。使用sublime打开文件，显示乱码，小编想把Mablab默认编码修改为UTF-8。

1、在Matlab安装目录下的bin目录下（例如`D:\Program Files\MATLAB\R2010b\bin`），找到lcdata.xml文件。

2、在matlab中，输入命令`feature('locale')`，查看当前使用的编码。

3、编辑lcdata.xml，找到如下一段：
```
<locale name="zh_CN" encoding="GB2312" xpg_name="zh_CN.GB2312">
    <alias name="zh-Hans"/>
</locale>
```

修改为：
```
<locale name="zh_CN" encoding="UTF-8" xpg_name="zh_CN.UTF-8">
    <alias name="zh-Hans"/>
</locale>
```

然而，修改后重启，matlab依然使用GBK编码。而且，把.m文件转换成UTF-8格式后，Matlab中会出现乱码。

无奈，从另一个方面入手，让sublime支持GBK编码文件，安装插件ConvertToUTF8即可。

# Matlab语言基础
## 变量和常量
matlab遵循弱类型语言的变量初始化和赋值语法，和python基本相同。

**输入数据**
`x = input('请输入数据')`，输入数据后，数据存入x。

**默认赋值**
如果输入数值，没有赋值给变量，那么默认赋值给内置的ans变量。

## 基本数据结构
**输入行矩阵**
`a = [1 2 3]`或`a = [1,2,3]`

**输入列矩阵**
`b = [1 2 3]'`或`b = [1,2,3]'`或`b = [1;2;3]`

**输入2*2矩阵**
`c = [1 2; 3 4]`

**输入特定值矩阵**
`d(2,3) = 8`，生成2*3的矩阵，第二行第三列的元素为8，其他元素为0。

**内置函数生成矩阵**
`ones(4)`，生成4*4的矩阵，所有元素为1。

`ones(4,3)`，生成4*3的矩阵，所有元素为1。

`zeros(4)`，生成4*4的矩阵，所有元素为0。

`zeros(4,3)`，生成4*3的矩阵，所有元素为0。

`eye(4)`，生成4*4的单位矩阵。

`eye(4,3)`，生成4*3的矩阵，前三行前三列组成单位矩阵，第四行为0。

**冒号表达式生成矩阵**
`3:9`，生成3到9的行向量，增量为1。

`3:2:9`，生成3到9的行向量，增量为2。

`(3:9)'`，生成3到9的列向量，增量为1。

**读取矩阵数据**
`a = [1 2 3]`，`a(2)`，读取第二个数据。

`c = [1 2; 3 4]`，`c(1,2)`，读取第一行第二列的数据。

`c(:,2)`，读取二列的数据。

`c(1,:)`，读取第一行的数据。

`k = [1 2 3;4 5 6;7 8 9;10 11 12]`，`k(2:4,2)`，读取第二列中第二行到第四行的数据。

**拼接矩阵**
`m = [c,c]`，横向拼接两个c矩阵。

`n = [c:c]`，纵向拼接两个c矩阵。

**矩阵函数**
`size(n)`，返回行列数。

`length(n)`，返回行数和列数中的较大者。


# 源码分享
https://github.com/voidking/matlab-start.git

# 后记

# 书签
Matlab视频教程
http://www.51zxw.net/list.aspx?cid=456



