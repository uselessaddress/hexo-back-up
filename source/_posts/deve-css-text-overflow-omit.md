title: CSS实现文本溢出显示省略号
toc: true
date: 2016-03-03 21:43:53
tags:
- css
- 前端
categories: 设计开发
---

# 单行文本溢出显示省略号

```
width: 300px; 
overflow: hidden; 
text-overflow: ellipsis; 
white-space: nowrap;
```

如果字符串长度超过300px，那么超出部分就变成`...`。

# 多行文本溢出显示省略号
```
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 3;
overflow: hidden;
```
因为使用了WebKit的CSS扩展属性，该方法适用于WebKit浏览器及移动端；

-webkit-line-clamp用来限制在一个块元素显示的文本的行数。为了实现该效果，它需要组合其他的WebKit属性。常见结合属性：
display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。
-webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。

# 怎样在js中判断文本是否溢出
问题描述：
一段文字限定行数，使用css把多余文字显示为省略号，请问怎么通过js判断这段文字是否有文字显示为省略号？

## 思路一
王晨帅哥提供了一个思路：取消css的-webkit-line-clamp属性，看看元素高度是否发生了变化，变化了就是有文字显示为省略号。

好想法，最后小编改进后如下：这段文字在页面上放两份，一份限定行数，正常显示；另一份不限定行数，隐藏起来。然后，对比这两段文字的高度是否相同。
具体实现：
```
// scss部分
.info{
    font-size: 1.2rem;
    margin-top: .6rem;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 4;
    overflow: hidden;
    p{
        &:not(:first-child){
            margin-top: .4rem;
        };  
        line-height: 1.8rem;
    }
}

.info-hidden{
    position: fixed;
    z-index: -10;
    visibility: hidden;
    font-size: 1.2rem;
    margin-top: .6rem;
    p{
        &:not(:first-child){
            margin-top: .4rem;
        };  
        line-height: 1.8rem;
    }
}
```

```
// js部分
var $info = $('.info');
var $info_hidden = $('.info-hidden');
if($info.height() === $info_hidden.height()){
    $('.more').hide();
}
```

## 思路二
后来张伟林帅哥提供了一个更好的思路：既然已经知道了限定的行数，那么判断一个高度就可以了。结合line-height，高度用scrollheight，判断scrollheight > line-height*你决定的行数。

小编马上搜索了一个scrollheight，发现，原来scrollHeight可以返回元素的完整高度。那么，比较一下scrollHeight和height不就可以了么？
具体实现：
```
// js部分
var $info = $('.info');
console.log($info.height());
console.log($info[0].scrollHeight);
if($info.height() >= $info[0].scrollHeight){
    $('.more').hide();
}
```

# 后记
利用js也可以实现文本溢出显示省略号，可以参考书签中的dotdotdot，很形象的名字。。。

# 书签
CSS实现单行、多行文本溢出显示省略号（…）
http://www.daqianduan.com/6179.html

jQuery.dotdotdot
http://www.bootcdn.cn/jQuery.dotdotdot/