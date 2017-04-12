---
title: 函数式编程之Scheme入门
toc: true
date: 2017-01-01 00:00:00
tags:
- 函数式编程
- lisp
- scheme
categories: 设计开发
---
# 前言
Lisp是Fortran语言之后第二古老的高级编程语言，自成立之初已发生了很大变化。如今，最广为人知的通用的是Lisp方言：Common Lisp和Scheme。

Common Lisp和Scheme有什么不同呢？个人认为，Common Lisp和Scheme，就像狮子和家犬。狮子更加强大，难以驯服；家犬更加小巧，容易驯服。而选择哪一种，取决于实际需要。鉴于小编只是需要学习一下函数式编程的思想，所以Scheme足够了。

PS：MIT 的两本著名教材 SICP（Structure and Interpretation of Computer Programs）和  HTDP（How to Design Programs）都是以Scheme为基础的。

<!--more-->

# 安装mit-scheme
1、进入[mit-scheme官网](https://www.gnu.org/software/mit-scheme/)，下载对应平台的mit-scheme（下文以windows为例）。

2、安装下载的mit-scheme（下文简称scheme）。

3、双击scheme快捷方式，如果出现如下界面，表明安装成功。
![](setup.jpg)

PS：如果打开scheme的时候报错“Requested allocation is too large. Try with smaller argument to --heap”。那么，在程序的快捷方式上右键，属性，在“目标”里加上`--heap 512`。 

4、把安装路径加入环境变量。
例如，我的scheme安装路径为`C:\Program Files (x86)\MIT-GNU Scheme`，那么，在path中加入`C:\Program Files (x86)\MIT-GNU Scheme\bin`。同时，新建系统变量MITSCHEME_LIBRARY_PATH，值为`C:\Program Files (x86)\MIT-GNU Scheme\lib`。

5、测试scheme。
打开命令行，输入`mit-scheme`，弹出如下界面，表明安装配置成功。
![](config.jpg)

6、退出scheme。
在scheme命令窗口输入`(exit)`，选择y，退出scheme命令窗口。

# hello world
在scheme命令窗口中输入代码非常麻烦，光标不能回退和上下移动，所以比较简单的方法就是运行已经写完的文件。
## 方法一
1、新建hello.scm文件，内容如下：
```
;The first program
(begin
    (display "Hello, World!")
    (newline)
)
```

2、运行hello.scm。
`mit-scheme -load hello.scm`，效果如下：
![](run.jpg)

PS：如果此时报错“Requested allocation is too large. Try with smaller argument to --heap”，那么，修改命令为`mit-scheme --heap 128 -load hello.scm`。

## 方法二
打开mit-scheme窗口后，`(cd "e:\\temp")`，`(load "hello.scm")`。

# 基本语法
## 数据类型
Scheme中的简单数据类型包含 boolean（布尔类型），number（数字类型），character（字符类型） 和 symbol（标识符类型）。

复合数据类型是以组合的方式通过组合其它数据类型数据来获得。包括string、vector、dotted pair、list等。

数据类型详解：
http://www.kancloud.cn/wizardforcel/teach-yourself-scheme/147165

## 代码结构
函数式编程只用“表达式”，不用“语句”。

“表达式”（expression）是一个单纯的运算过程，总是有返回值；“语句”（statement）是执行某种操作，没有返回值。函数式编程要求，只使用表达式，不使用语句。也就是说，每一步都是单纯的运算，而且都有返回值。

原因是函数式编程的开发动机，一开始就是为了处理运算（computation），不考虑系统的读写（I/O）。"语句"属于对系统的读写操作，所以就被排斥在外。
当然，实际应用中，不做I/O是不可能的。因此，编程过程中，函数式编程只要求把I/O限制到最小，不要有不必要的读写行为，保持计算过程的单纯性。

基本格式：
`(function_name arg0,arg1,...)`

示例：
```
(display "Hello, World!")
(newline)
(exit)
(number? 42)
(eqv? 42 42)
(>= 4.5 3)
```

迄今为止我们提供的Scheme示例程序都是s-表达式。这对所有的Scheme程序来说都适用：程序是数据。

Scheme运行一个列表形式的代码结构时，首先要检测列表第一个元素，或列表头。如果这个列表头是一个过程，则代码结构的其余部分则被当成将传递给这个过程的参数集，而这个过程将接收这些参数并运算。

如果这个代码结构的列表头是一个特殊的代码结构，则将会采用一种特殊的方式来运行。常见的特殊的代码结构有begin， define和 set!。

begin可以让它的子结构可以有序的运算，而最后一个子结构的结果将成为整个代码结构的运行结果。define会声明并会初始化一个变量。set! 可以给已经存在的变量重新赋值。

## 过程（函数）定义

### 匿名过程
我们已经见过了许多系统过程，比如，cons， string->list等。用户可以使用代码结构lambda来创建自定义的过程。例如，下面定义了一个过程可以在它的参数上加上2：
```
(lambda (x) (+ x 2))
```

第一个子结构，(x)，是参数列表。其余的子结构则构成了这个过程执行体。这个过程可以像系统过程一样，通过传递一个参数完成调用：
```
((lambda (x) (+ x 2)) 5)
```


### 一般定义
如果我们希望能够多次调用这个相同的过程，我们可以每次使用lambda重新创建一个复制品，但我们有更好的方式。我们可以使用一个变量来承载这个过程：
```
(define add2
  (lambda (x) (+ x 2)))
```

只要需要，我们就可以反复使用add2为参数加上2：
```
(add2 4) =>  6
(add2 9) =>  11
```
定义过程还可以有另一种简单的方式，直接用define而不使用lambda来创建：
```
(define (add2 x)
       (+ x 2))
```

### 过程的参数
lambda 过程的参数由它的第一个子结构（紧跟着lambda标记的那个结构）来定义。add2是一个单参数或一元过程，所以它的参数列表是只有一个元素的列表(x)。标记x作为一个承载过程参数的变量而存在。在过程体中出现的所有x都是指代这个过程的参数。对这个过程体来说x是一个局部变量。

我们可以为两个参数的过程提供两个元素的列表做参数，通常都是为n个参数的过程提供n个元素的列表。下面是一个可以计算矩形面积的双参数过程。它的两个参数分别是矩形的长和宽。
```
(define area
  (lambda (length breadth)
    (* length breadth)))
```
我们看到area将它的参数进行相乘，系统过程*也可以实现相乘。我们可以简单的这样做：
```
(define area *)
```

## apply过程
apply过程允许我们直接传递一个装有参数的list 给一个过程来完成对这个过程的批量操作。
```
(define x '(1 2 3))

(apply + x)
=>  6
```

通常，apply需要传递一个过程给它，后面紧接着是不定长参数，但最后一个参数值一定要是list。它会根据最后一个参数和中间其它的参数来构建参数列表。然后返回根据这个参数列表来调用过程得到的结果。例如:
```
(apply + 1 2 3 x)
=>  12
```

## 顺序执行
我们使用begin这个特殊的结构来对一组需要有序执行的子结构来进行打包。许多Scheme的代码结构都隐含了begin。例如，我们定义一个三个参数的过程来输出它们，并用空格间格。一种正确的定义是：
```
(define display3
  (lambda (arg1 arg2 arg3)
    (begin
      (display arg1)
      (display " ")
      (display arg2)
      (display " ")
      (display arg3)
      (newline))))
```
在Scheme中，lambda的语句体都是隐式的begin代码结构。因此，display3语句体中的begin不是必须的，不写时也不会有什么影响。

display3更简化的写法是：
```
(define display3
  (lambda (arg1 arg2 arg3)
    (display arg1)
    (display " ")
    (display arg2)
    (display " ")
    (display arg3)
    (newline)))
```

## 条件语句
和其它的编程语句一样，Scheme 也包含条件语句。

### if
最基本的结构就是if：
```
(if 测试条件
    then-分支
    else-分支)
```
如果测试条件运算的结果是真(即，非#f的任何其它值)，then分支将会被运行(即满足条件时的运行分支)。否则，else分支会被运行。else分支是可选的。
```
(define p 80)

(if (> p 70) 
    'safe
    'unsafe)
=>  safe 

(if (< p 90)
    'low-pressure) ;no ``else'' branch
=>  low-pressure 
```
为了方便，Scheme还提供了一些其它的条件结构语句。它们可以被定义成宏来扩充if表达式。

### when 和 unless
当我们只需要一个基本条件语句分支时（”then”分支或”else”分支），使用when 和 unless会更方便。
```
(define a 10)
(define b 20)
(when (< a b)
    (display "a是")
    (display a)
    (display "b是")
    (display b)
    (display "a小于b" ) )
```
先判断a是否小于b，这个条件成立时会输出倒序的5条信息。

同样的功能还可以像下面这样用unless来写(unless和when的意思正好相反)：
```
(define a 10)
(define b 20)
(unless (>= a b)
   (display "a是")
   (display a)
   (display "b是")
   (display b)
   (display "a小于b" ) )
```
并不是所有的Scheme环境都提供when和unless。
如果你的Scheme中没有，你可以用宏来自定义出when和unless。

使用if实现相同的程序会是这样：
```
(define a 10)
(define b 20)
(if (< a b)
    (begin
        (display "a是")
        (display a)
        (display "b是")
        (display b)
        (display "a小于b" ) ))
```
先判断a是否小于b，这个条件成立时会输出正序的5条信息。

在mit-scheme下，中文会显示乱码，为方便查看，建议修改成英文。

### cond

cond结构在表示多重if表达式时很方便，
多重if结构除了最后一个else分支以外的其余分支都会包含一个新的if条件。因此，
```
(if (char<? c #\c) -1
    (if (char=? c #\c) 0
        1))
```
这样的结构都可以使用cond来这样写：
```
(cond ((char<? c #\c) -1)
    ((char=? c #\c) 0)
    (else 1))
```
cond就是这样的一种多分支条件结构。每个从句都包含一个判断条件和一个相关的操作。第一个判断成立的从句将会引发它相关的操作执行。如果任何一个分支的条件判断都不成立则最后一个else分支将会执行(else分支语句是可选的)。

cond的分支操作都是begin结构。

### case
当cond结构的每个测试条件是一个测试条件的分支条件时，可以缩减为一个case表达式。
```
(define c #\c)
(case c
    ((#\a) 1)
    ((#\b) 2)
    ((#\c) 3)
    (else 4))
```
分支头值是#\c 的分支将被执行。

### and 和 or

Scheme提供了对boolean值进行逻辑与and和逻辑或or运算的结构。(我们已经见过了布尔类型的求反运算not过程。)

当所有子结构的值都是真时，and的返回值是真，实际上，and的运行结果是最后一个子结构的值。如果任何一个子结构的值都是假，则返回#f。
```
(and 1 2)  =>  2
(and #f 1) =>  #f
```
而or会返回它第一个为值为真的子结构的结果。如果所有的子结构的值都为假，or则返回#f。
```
(or 1 2)  =>  1
(or #f 1) =>  1
```
and和or都是从左向右运算。当某个子结构可以决定最终结果时，and和or会忽略剩余的子结构，即它们是“短路”的。
```
(and 1 #f expression-guaranteed-to-cause-error)
=>  #f

(or 1 #f expression-guaranteed-to-cause-error)
=>  1
```



# 源码分享
https://github.com/voidking/scheme-start.git

# 书签
函数式编程初探
http://www.ruanyifeng.com/blog/2012/04/functional_programming.html

函数式编程入门教程
http://www.ruanyifeng.com/blog/2017/02/fp-tutorial.html

Lisp教程
http://www.yiibai.com/lisp/

Scheme语言简明教程
http://www.kancloud.cn/wizardforcel/teach-yourself-scheme/147161

MIT/GNU Scheme Documentation
http://www.math.pku.edu.cn/teachers/qiuzy/progtech/scheme/MIT_Scheme_doc/index.html

符号: 抽象、语义
http://mp.weixin.qq.com/s/EeP9lggM5GKNF-UmvkTEqA

编程语言的基石——Lambda calculus
http://liujiacai.net/blog/2014/10/12/lambda-calculus-introduction/



