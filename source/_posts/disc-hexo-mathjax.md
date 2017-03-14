---
title: Hexo中使用Mathjax的冲突问题
toc: true
date: 2017-03-14 16:00:00
tags:
- hexo
- mathjax
categories: 点滴发现
---
# 问题描述
写《信息量&信息熵&信息增益》时，用到了一些数学公式。但是，在使用hexo生成页面后，这些数学公式并没有被正确显示。查找资料发现markdwon本身的特殊符号与latex中的符号会出现冲突:

1、在markdown中，`_`是斜体，但是在latex中，却有下标的意思。
2、在markdown中，`\\`会被转义为`\`,这样也会影响影响mathjax对公式中的`\\`进行渲染。

解决办法有三种，手动添加转义、更换hexo的markdown渲染引擎和修改hexo的渲染代码。小编更喜欢第三种，经过尝试，完美解决问题，本文记录下解决问题的过程。

<!--more-->

# 解决问题
## 确认开启mathjax
以小编使用的yilia主题为例，主题本身已经集成了mathjax，其他主题可以参考添加。

在`yilia\layout\_partial\after_footer.ejs` 里有如下代码：
```
<% if (theme.mathjax){ %>
<%- partial('mathjax') %>
<% } %>
```
当_config.yml文件中的mathjax参数为true时，页面中便会引入mathjax.ejs。

在`yilia\layout\_partial\mathjax.ejs`里有如下代码：

```
<! -- mathjax config similar to math.stackexchange -->

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [ ['$','$'], ["\\(","\\)"]  ],
        processEscapes: true,
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
    }
});

MathJax.Hub.Queue(function() {
    var all = MathJax.Hub.getAllJax(), i;
    for(i=0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';                 
    }       
});
</script>

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
```
引入了mathjax，以及配置mathjax。


其实更好的做法，是把after_footer.ejs中的代码修改为：
```
<% if (page.mathjax){ %>
<%- partial('mathjax') %>
<% } %>
```
这样，是否在页面中引入mathjax，就交由了页面控制，更加灵活。
写文档时，在头部加入`mathjax:true`，或者`mathjax:false`。

```
---
title: Hexe中使用Mathjax的冲突问题
toc: true
mathjax: false
date: 2017-03-14 16:00:00
tags:
- hexo
- mathjax
categories: 点滴发现
---
```


## 修改渲染代码
找到`hexo\node_modules\hexo-renderer-marked\node_modules\marked\lib\markd.js`，备份后进行修改：

**1、去掉`\`的转义**
```
escape: /^\\([\\`*{}\[\]()# +\-.!_>])/,
```
修改为
```
escape: /^\\([`*{}\[\]()# +\-.!_>])/,
```

**2、去掉`_`的斜体含义**
```
em: /^\b_((?:[^_]|__)+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```
修改为
```
em:/^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```


# 书签
MathJax
https://www.mathjax.org/

期刊文章 - TeXIDE, 在线 LaTeX 编辑器
https://www.texide.com/templates/journals/?

JaxEdit Website
http://jaxedit.com/

Cmd Markdown 编辑阅读器 - 作业部落出品
https://www.zybuluo.com/mdeditor

在 Hexo 中完美使用 Mathjax 输出数学公式
http://lukang.me/2014/mathjax-for-hexo.html

Hexo下mathjax的转义问题
https://segmentfault.com/a/1190000007261752