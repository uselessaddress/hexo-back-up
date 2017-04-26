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

`magic(4)`，生成4*4的魔方数组。

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


## 空数组和子数组
**空数组**
`nullmatrix = []`，创建空数组。


**子数组**
`magicmatrix = magic(4)`，`child = magicmatrix(3,[2,4])`

`child = magicmatrix(3,2:end)`

`magicmatrix(3,2) = 3`，单个元素赋值。

**等差数列**
`linspace(1,99,50)`，生成1到99的50个数，等差为2。

**等比数列**
`logspace(1,3,3)`，生成10 100 1000三个数。


**数组变形**
`1:1:9`，`reshape(ans,3,3)`，按列排列变形成为方数组。

## 时间变量
`data`，`clock`，`year(now)`，`month(now)`，`day(now)`，`day(today)`

## 算术运算符

**标量运算**
正常加减乘除，和python基本相同。需要注意的是`5/6`代表5除以6，`5\6`代表6除以5。

`5^2`和`power(5,2)`都是代表5的平方。

**矩阵运算**
`A=[1 2 3; 4 5 6; 7 8 9]`，`B=magic(3)`

`A+B`，`A-B`，`A*B`，`A+3`，`A*3`

`inv(B)`，求B矩阵的逆矩阵。

`A/B`和`A*inv(B)`相同。

`A.*B`，对应位置相乘。

## 运算函数
matlab的运算函数包括幂次方、指数与对数函数、三角与反三角函数等等。

**幂次方**
幂次方的符号就是常用的^记号。指数部分可以是任意数。
`2^2`，`2^(-1)`，`2^(1/2)`，`2^(1.25)` 

**指数与对数**
科学与工程领域惯用「标准指数函数」，也就是以e为底的指数函数。其中，e是一个无理数，大约等于2.71828。Matlab并不提供e这个常数，而是以函数exp()来计算以e为底的指数函数。
`exp(1)`

Matlab 分别提供三个函数 log()、log10()和log2()，分别表示以e为底的对数（自然对数），以10为底
的对数（常用对数），以2为底的对数。
`log(exp(2))`，`log10(100)`，`log2(4)`，答案都是2。

**三角与反三角函数**
六个三角函数在Matlab 中对应的函数分别为：
正弦：sin()
余弦：cos()
正切：tan()
余切：cot()
正割：sec()
余割：csc()

六个反三角函数在Matlab 中对应的函数分别为：
反正弦：asin()
反余弦：acos()
反正切：atan()
反余切：acot()
反正割：asec()
反余割：acsc()

需要注意的就是使用三角函数时，角度的单位是“弧度”。

**复数**
Matlab 的所有运算符号、所有函数，都懂得如何做复数计算。
`sqrt(-1)`

`abs(3+4i)`

## 微积分
**极限**
求极限是微积分的基础，求极限的函数limit。
`limit(f,x,a)`，x趋近于a时，f 的极限。
`limit(f,x,a,'left')`，x左趋近于a时，f 的极限。
`limit(f,x,a,'right')`，x右趋近于a时，f的极限。

**微分**
`diff(f,t,n)`，求f 对独立变量t的n次微分值。

**积分**
`int(f,'t',a,b)`，求f 对独立变量t 在积分区间[a，b]的积分值。

**级数**
自变量v在[a，b]之间取值时，对通项s求和，用函数`symsum(s,v,a,b)`。

## 方程求解
**代数方程**
`solve(f)`

`solve(f,a)`

**常微分方程**
`dsolve('常微分方程式','初始条件','自变量')`

# 可视化
## 二维平面图形

### 折线图
plot(x,y)函数，x，y是维度相同的序列或向量。
```
x=[0 1 2];
y=[0 1 0];
plot(x,y);
```

用300段折线画出sin(x)在[-pi,pi]区间内的折线图。
```
x = linspace(-pi, pi, 301);
plot(x, sin(x));
```

如果要画多条曲线，也可以用plot函数。
```
x=0:pi/10:2*pi;
y1=sin(x);
y2=cos(x);
plot(x,y1,x,y2);
plot(x,y1,'r + -',x,y2,'k * :');
```
图形是以公共的x元素为横坐标值，y1、y2为纵坐标值绘制曲线图的。如果想要图形更加完美，我们可以用一些特殊的图形函数对它进行修饰。

```
xlabel('独立变量X');
ylabel('变量Y');
title('正弦和余弦曲线');
text(1.5,0.3,'cos(x)');
text(0,0,'sin(x)');
%axis([0 2*pi -0.9 0.9]);
```

如果只给plot()一个参数，例如plot(y)，而y是一个n维向量或列，则它的效果就相当于plot((1:n), y)。
```
y = [ 1 4 0 2 3 5];
plot(y);
```

### 多重折线图

Matlab在一张图片上可以重复制图。基本上，画一张图的指令，将会自动清除前一张图。但是，如果下了指令hold on，将不会清除前一张图，而是重复画上去。下了hold on指令的所有图将会重迭在一张图片里，直到你下了hold off为止。

我们以 300 个折线段，在一张图片中，画出以下三个函数在[-pi, pi]区间内的曲线图： 
sin(x)，cos(x)，x。
```
x = linspace(0, 2*pi, 301);
y = sin(x);
plot(x, y, 'r');
axis([0 2*pi -1.2 1.2]);
hold on
y = cos(x);
plot(x, y, 'g');
y = x; 
plot(x, y, 'b');
hold off;
```

我们还可以采用图形窗口分割的方法，在同一个视图窗口中画出多个小图形。这时要用到subplot(n,m,k)。如果写subplot(2,2,1),即就是把图形窗口分割成2行2列，在第1个位置（第1行第1列）画图。
```
x = linspace(0, 2*pi, 301);
y = sin(x);
subplot(2,2,1);
plot(x, y);
y = cos(x);
subplot(2,2,2);
plot(x, y);
```
Matlab对数据是按列存储和计算的。

## 三维立体图形
### 三维曲线图
plot3函数调用格式：plot3(x1,y1,z1,x2,y2,z2,…)。其中x1，y1，z1，x2，y2，z2…等分别为维数相同的向量，分别存储着曲线的三个坐标值。

绘制方程在t=[0,2π]的空间方程。

\begin{cases}
x=t \\ 
y=sin(t) \\ 
z=cos(t)
\end{cases}

```
t=0:pi/10:2*pi;
x=t;
y=sin(t);
z=cos(t);
plot3(x,y,z,'r:p');
grid on;
xlabel('X');
ylabel('Y');
zlabel('Z');
title('sine and cosine');
```

### 三维网格图和曲面图
Matlab在绘制三维网格图与曲面图时，往往先将要绘制图形的定义区域分成若干网格，然后计算这些网格节点上的二元函数值，最后才能使用mesh和surf函数绘制相应的图形。生成网格矩阵使用meshgrid函数，其调用格式为：
```
[U, V]=meshgrid(x,y)
```

函数说明：利用向量x和y生成网格矩阵U和V，以便mesh和surf等函数用来绘图。其中x、y分别是长度为n和m升序排列的行向量。

生成的方法是将x复制n次生成网格矩阵U，将y转置成列向量后复制m次生成网格矩阵V。坐标(uij,vij)表示xoy平面上网格节点的坐标，第三维坐标zij=f(uij,vij)。

例：给定向量x=[1 2 3 4]，y=[10 11 12 13 14]，试由向量x、y生成网格矩阵。

```
x=[1 2 3 4]; %输入向量x
y=[10 11 12 13 14]; %输入向量y
[U,V]=meshgrid(x,y); %生成网格矩阵
Z=peaks(U,V);
mesh(U,V,Z); %绘制三维网格图
```

Matlab提供了一个peaks函数，可产生一个凹凸有致的曲面，包含了三个局部极大点及三个局部极小点。在matlab中输入`peaks()`、`peaks(5)`就可以看到效果。

例：在 `-4<x<4，-4<y<4` 上绘制 $z=x^2+y^2$ 的三维网格图。

```
[x,y]=meshgrid(-4:4, -4:4); %定义网格数据向量x,y
z=x.^2+y.^2; %计算二元函数值
mesh(x,y,z); %绘制三维网格图
% surf(x,y,z); %绘制三维曲面图
```

### 观察点
函数view(azinmuth,elevation)
azinmuth：方位角。观察点与坐标原点的连线在水平面上的投影和y轴负方向的夹角。（在水平面上）
elevation：仰角。观察点与坐标原点的连线和水平面的夹角。（与水平面垂直）

使用循环和观察点设定来实现动画效果。

# Matlab程序设计
## 命令文件
Matlab提供两种源程序文件格式：命令文件和函数文件。这两种文件的扩展名相同，均为“.m”，又称为“M文件”。
命令文件的执行方式：在提示符后键入命令文件的文件名。
命令文件适合于用户做需要理解得到结果的小规模运算。

## 函数文件
函数文件由function语句引导。
其格式为：
```
function [返回变量列表]=函数名(输入变量列表)
```

1、新建一个求阶乘的函数文件myFunc.m：
```
function value = myFunc(n);

if n<=1
    value = 1;
else
    value = myFunc(n-1)*n;
end
```

2、重写求阶乘的函数文件myFunc2.m：
```
function value = myFunc2(n);

value = 1;
while n > 1
    value = value*n;
    n = n - 1;
end
```

3、重写求阶乘的函数文件myFunc3.m：
```
function value = myFunc3(n);

value = 1;
for i=1:1:n
    value = value*i;
end

%for i=n:-1:1
%    value = value*i;
%end
```

4、新建传入数值显示结果的函数文件showNum.m：
```
function showNum(input_var);

switch input_var
    case 1
        disp('1');
    case {2,3,4}
        disp('2 or 3 or 4');
    case 5
        disp('5');
    otherwise
        disp('something else');
end
```

# 源码分享
https://github.com/voidking/matlab-start.git

# 书签
Matlab视频教程
http://www.51zxw.net/list.aspx?cid=456

我的学习资料
http://pan.baidu.com/s/1bp1oyXT

Mathjax与LaTex公式简介
http://mlworks.cn/posts/introduction-to-mathjax-and-latex-expression/


