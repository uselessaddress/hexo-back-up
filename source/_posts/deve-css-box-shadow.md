---
title: css阴影效果
toc: false
date: 2016-07-13 10:50:25
tags:
- css
- 前端
categories: 设计开发
---

有阴影的图，看上去高大上些？不管怎样，UI设计了阴影，咱就照做好了。

语法：
```
box-shadow: h-shadow v-shadow blur spread color inset;
```

<!--more-->

解释：

| 值 | 描述 | 
| ----- | ----- |
| h-shadow | 必需。水平阴影的位置。允许负值。 |
| v-shadow | 必需。垂直阴影的位置。允许负值。 |
| blur | 可选。模糊距离。 |
| spread | 可选。阴影的尺寸。 |
| color | 可选。阴影的颜色。请参阅 CSS 颜色值。 |
| inset | 可选。将外部阴影 (outset) 改为内部阴影。 |

常见用法：
```
div{
    box-shadow: 10px 10px 5px #0cc;
}
```
四个值分别是水平阴影位置、垂直阴影位置、模糊距离、颜色。

实际案例：
```
<a href="" class="confirm">
    <span>马上去抢2G流量</span>
</a>
```

```
.confirm{
    display: inline-block;
    box-shadow: 0 .5rem 1.5rem #0cc;
    border-radius: 5px;
    margin-top: 12%;
    width: 98%;
    height: 17%;
    background: url(../../img/flowrate/blue.jpg) no-repeat;
    background-size: 100% 100%;
    color: #fff;
    span{
        display: inline-block;
        margin-top: 3%;
        font-size: 1.5rem;
    }
}
```

最终效果如下：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/box-shadow/shadow.jpg)
