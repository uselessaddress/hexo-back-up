title: 如何引入另一个html文件
date: 2014-09-24 12:52:21
tags:
- html
categories: 设计开发
---

### iframe方式
```

<iframe name="content_frame" width=100% height=30 marginwidth=0 marginheight=0 src="import.htm" mce_src="import.htm" ></iframe>

```
<!--more-->
你会看到一个外部引入的文件，但会发现有一个类似外框的东西将其包围，可使用
```
<iframe name="content_frame" marginwidth=0 marginheight=0 width=100% height=30 src="import.htm" mce_src="import.htm" frameborder=0></iframe>
```

但你会发现还会有点问题，就是背景色不同，你只要在引入的文件import.htm中使用相同的背景色也可以
如果想引入的文件过长时不出现滚动条的话在import.htm中的body中加入scroll=no

### object方式
```
<object style="border:0px" type="text/x-scriptlet" data="import.htm" width=100% height=30></object>
```

### behavior的download方式

```
<span id=showImport></span>
<IE:Download ID="oDownload" STYLE="behavior:url(#default#download)" />
<script>
function onDownloadDone(downDate){
	showImport.innerHTML=downDate
}
oDownload.startDownload(@#import.htm@#,onDownloadDone)
</script>

```