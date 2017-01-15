---
title: Python基础
toc: true
date: 2017-01-12 21:00:52
tags:
- python
categories: 设计开发
---
# 前言
Python，是龟叔在1989年为了打发无聊的圣诞节而编写的一门编程语言，特点是优雅、明确、简单，现今拥有丰富的标准库和第三方库。
Python适合开发Web网站和各种网络服务，系统工具和脚本，作为“胶水”语言把其他语言开发的模块包装起来使用，科学计算等等。

小编学习Python的理由有三个：
为了爬取需要的各种数据，不妨学习一下Python。
为了分析数据和挖掘数据，不妨学习一下Python。
为了做一些好玩有趣的事，不妨学习一下Python。

<!--more-->
# 准备工作
1、在Python官网下载安装喜欢的版本，小编使用的，是当前最新版本3.6.0。
2、打开IDLE，这是Python的集成开发环境，尽管简单，但极其有用。IDLE包括一个能够利用颜色突出显示语法的编辑器、一个调试工具、Python Shell，以及一个完整的Python3在线文档集。

# hello world
1、在IDLE中，输入`print('hello world')`，回车，则打印出hello world。
PS：语句末尾加不加分号`;`都可以，小编决定不加分号，更简单。

2、使用sublime新建文件hello.py，内容如下：
```
print('hello world')
```
在Windows下，shift+右键，在此处打开命令窗口，执行`python hello.py`，回车，则打印出hello world。

3、使用sublime新建文件hello.py，内容如下：
```
#!/usr/bin/env python
print('hello world')
```
在Linux或Mac环境下，可以直接运行脚本。首先添加执行权限`chmod a+x hello.py`，然后执行`./hello.py`。当然，也可以和Windows一样，使用`python hello.py`来执行脚本。

# 引入模块
1、新建name.py，内容如下：
```
name='voidking'
```
2、执行`python name.py`。
3、进入python shell模式，执行`import name`，`print(name.name)`，则打印出voidking。

# 基础语法
常用函数（print）、数据类型、表达式、变量、条件和循环、函数。和其他语言类似，下面选择一部分展开。

# list链表数组
1、定义数组
`myList = ['Hello', 100, True]`
2、输出数组
`print(myList)`
3、输出数组元素
`print(myList[0])`，`print(myList[-1])`
4、追加元素到末尾
`myList.append('voidking')`
5、追加元素到头部
`myList.insert(0,'voidking')`
6、删除元素
`myList.pop()`，`myList.pop(0)`
7、元素赋值
`myList[0]='hello666'`

# tuple固定数组
1、定义数组
`myTuple = ('Hello', 100, True)`
错误定义：`myTuple1=(1)`，正确定义：`myTuple=(1,)`
2、输出数组
`print(myTuple)`
3、输出数组元素
`print(myTuple[0])`
4、tuple和list结合
`t = ('a', 'b', ['A', 'B'])`，`t[2][0]='X'`

# if语句
## if
```
score = 75
if score>=60:
    print 'passed'
```
两次回车，即可执行代码。

## if-else
```
if score>=60:
    print('passed')
else:
    print('failed')
```

## if-elif-else
```
if score>=90:
    print('excellent')
elif score>=80:
    print('good')
elif score>=60:
    print('passed')
else:
    print('failed')
```

# 循环
## for循环
```
L = [75, 92, 59, 68]
sum = 0.0
for score in L:
       sum += score
print(sum / 4)
```

## while循环
```
sum = 0
x = 1
while x<100:
    sum += x
    x = x + 1
print(sum)
```

## break
```
sum = 0
x = 1
while True:
    sum = sum + x
    x = x + 1
    if x > 100:
        break
print(sum)
```

## continue
```
L = [75, 98, 59, 81, 66, 43, 69, 85]
sum = 0.0
n = 0
for x in L:
    if x < 60:
        continue
    sum = sum + x
    n = n + 1
print(sum/n)
```

## 多重循环
```
for x in ['A', 'B', 'C']:
    for y in ['1', '2', '3']:
        print(x + y)
```

# dict
dict的作用是建立一组 key和一组value的映射关系。
```
d = {
    'Adam': 95,
    'Lisa': 85,
    'Bart': 59,
    'Paul': 75
}
print(d)
print(d['Adam'])
print(d.get('Lisa'))
d['voidking']=100
print(d)
for key in d:
    print(key+':',d.get(key))
```

# set
set持有一系列元素，这一点和list很像，但是set的元素没有重复，而且是无序的，这点和dict的key很像。
```
s = set(['Adam', 'Lisa', 'Bart', 'Paul'])
print(s)
s = set(['Adam', 'Lisa', 'Bart', 'Paul', 'Paul'])
print(s)
len(s)
print('Adam' in s)
print('adam' in s)
for name in s:
    print(name)
```

```
s = set([('Adam', 95), ('Lisa', 85), ('Bart', 59)])
for x in s:
    print(x[0]+':',x[1])
```

```
s.add(100)
print(s)
s.remove(('Adam',95))
print(s)
```

# 函数
## 自带函数
```
del sum
L = [x*x for x in range(1,101)]
print sum(L)
```

## 自定义函数
```
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
my_abs(-100)
```

## 引入函数库
```
import math

def quadratic_equation(a, b, c):
    x = b * b - 4 * a * c
    if x < 0:
        return none
    elif x == 0:
        return -b / (2 *a)
    else:
        return ((math.sqrt(x) - b ) / (2 * a)) , ((-math.sqrt(x) - b ) / (2 * a))
print(quadratic_equation(2, 3, 0))
print(quadratic_equation(1, -6, 5))
```

## 可变参数
```
def average(*args):
    if args:
        return sum(args)*1.0/len(args)
    else:
        return 0.0

print(average())
print(average(1, 2))
print(average(1, 2, 2, 3, 4))
```

# 切片
## list切片
```
L = ['Adam', 'Lisa', 'Bart', 'Paul']
L[0:3]
L[:3]
L[1:3]
L[:]
L[::2]
```

## 倒序切片
```
L[-2:]
L[-3:-1]
L[-4:-1:2]
```

```
L = range(1, 101)
L[-10:]
L[4::5][-10:]
```
PS：range是有序的list，默认以函数形式表示，执行range函数，即可以list形式表示。

## 字符串切片
```
def firstCharUpper(s):
    return s[0:1].upper() + s[1:]

print(firstCharUpper('hello'))
```

# 迭代
Python的for循环不仅可以用在list或tuple上，还可以作用在其他任何可迭代对象上。
迭代操作就是对于一个集合，无论该集合是有序还是无序，我们用for循环总是可以依次取出集合的每一个元素。
集合是指包含一组元素的数据结构，包括：
- 有序集合：list，tuple，str和unicode；
- 无序集合：set
- 无序集合并且具有key-value对：dict

```
for i in range(1,101):
    if i%7 == 0:
        print(i)
```

## 索引迭代
对于有序集合，元素是有索引的，如果我们想在for循环中拿到索引，怎么办？方法是使用enumerate()函数。
```
L = ['Adam', 'Lisa', 'Bart', 'Paul']
for index, name in enumerate(L):
    print(index+1, '-', name)

myList = zip([100,20,30,40],L);
for index, name in myList:
    print(index, '-', name)
```

## 迭代dict的value
```
d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59 }
print(d.values())
for v in d.values():
    print(v)
```

PS：Python3.x中，dict的方法dict.keys()，dict.items()，dict.values()不会再返回列表，而是返回一个易读的“views”。这样一来，`k = d.keys();k.sort()`不再有用，可以使用`k = sorted(d)`来代替。
同时，dict.iterkeys()，dict.iteritems()，dict.itervalues()方法不再支持。

## 迭代dict的key和value
```
d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59 }
for key, value in d.items():
    print(key, ':', value)
```

# 列表生成
## 一般表达式
```
L = [x*(x+1) for x in range(1,100)]
print(L)
```

## 复杂表达式
```
d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59 }
def generate_tr(name, score):
    if score >=60:
        return '<tr><td>%s</td><td>%s</td></tr>' % (name, score)
    else:
        return '<tr><td>%s</td><td style="color:red">%s</td></tr>' % (name, score)

tds = [generate_tr(name,score) for name, score in d.items()]
print('<table border="1">')
print('<tr><th>Name</th><th>Score</th><tr>')
print('\n'.join(tds))
print('</table>')
```

## 条件表达式
```
L = [x * x for x in range(1, 11) if x % 2 == 0]
print(L)
```

```
def toUppers(L):
    return [x.upper() for x in L if isinstance(x,str)]

print(toUppers(['Hello', 'world', 101]))
```

## 多层表达式
```
L = [m + n for m in 'ABC' for n in '123']
print(L)
```

```
L = [a*100+b*10+c for a in range(1,10) for b in range(0,10) for c in range(1,10) if a==c]
print(L)
```

# 后记
至此，Python基础结束。接下来，爬虫飞起！

# 书签
Python官网
https://www.python.org/

Python入门
http://www.imooc.com/learn/177

如何学习Python爬虫[入门篇]？
https://zhuanlan.zhihu.com/p/21479334?refer=passer

你需要这些：Python3.x爬虫学习资料整理
https://zhuanlan.zhihu.com/p/24358829?refer=passer