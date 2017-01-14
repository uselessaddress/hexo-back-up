title: eclipse实用设置
date: 2014-02-17 20:10:24
tags: 
- eclipse
categories: 设计开发
toc: true
---


### 设置项目默认编码
在eclipse中，Window，Preference，General，Workspace，Text file encoding，Other选择UTF-8。

### 设置JSP页面默认编码
在eclipse中，Window，Preference，Web，JSP Files，Encoding选择ISO 10646/Uincode(UTF-8)。

### 设置代码提示
在eclipse中，Window，Preference，Java，Editor，Content Assist，Auto Activation，Auto activation triggers for Java中输入“.”和52个英文字符。

### 设置快捷键
在eclipse中，Window，Preference，General，Keys。
<!--more-->
### eclipse查看原始类
例如：eclipse查看原始类出现The jar file rt.jar has no source attachment。
解决办法：Window，Preferences，Java，Installed JREs，Edit，JREsystem libraries中选中rt.jar，Source Attachment...，选中src.zip。

### 代码提示上屏设置
例如：要新建一个String类型的变量value，则当输入到value的时候，eclipse会在候选列表中列出valueString，如果此时再输入空格的话，就会选中候选列表中的valueString，则新建的变量将会变成valueString。

解决办法：打开eclipse，Window，Show View，Plug-ins，找到org.eclipse.jface.text右击，import as，Source Project，导入完成后，在你的workspace就可以看到这个project了。

PS：修改插件，需要下载**相同Version**的Eclipse RCP（该版本修改源代码比较方便，能自动导入源代码），因为开发版、企业版等导出Plug-ins项目后无法编辑。

然后，在导入工程下的src文件夹下，找到包org.eclipse.jface.text.contentassist，类CompletionProposalPopup，函数verifyKey()。
```
if (contains(triggers, key)) {
	e.doit= false;
	hide();
	insertProposal(p, key, e.stateMask, fContentAssistSubjectControlAdapter.getSelectedRange().x);
}
```
修改为
```
if (key != '=' && key != 0x20 && key != ';' && contains(triggers, key)) {
	e.doit= false;
	hide();
	insertProposal(p, key, e.stateMask, fContentAssistSubjectControlAdapter.getSelectedRange().x);
}
```
实现效果：等号键、空格键、分号键不上屏。

```
case '\t':
	e.doit= false;
	fProposalShell.setFocus();
	return false;
```
修改为
```
case '\t':
	e.doit= false;
	insertSelectedProposalWithMask(e.stateMask);
	return false;
```
实现效果：Tab键上屏。

修改完成后，项目右击，Export，Deployable plug-ins and fragments，勾选Available Plug-ins and Fragments，指定Destination的Directory，Finish。

使用导出的jar包，替换原eclipse/plugins下的jar包，即可实现插件的修改。

### 2016.11.25更新
最新版的eclipse，Neon.1a Release (4.6.1)，插件的所在目录变更如下：
`C:\Users\Administrator\.p2\pool\plugins\`，使用导出的jar包，替换该目录下的jar包，即可实现插件的修改。

如果再次变更插件所在目录，请依次点击Window，Show View，Plug-ins。然后单击任意插件，在eclipse底部即可看到插件路径。

### 带源文件的jar包下载
http://www.java2s.com/Code/Jar/CatalogJar.htm



### 配置本地dtd文件

我们以配置xwork-validator-1.0.dtd文件为例：

1、解压xwork-core-*.jar包，找到xwork-validator-1.0.dtd。

2、eclipse，Window，Preferences，XML，XML Catelog，Add，File System...，选中刚才解压的xwork-validator-1.0.dtd，打开，

3、Location已经选好`D:\jar\struts2\xwork-validator-1.0.dtd`，Key type选择`Public ID`，Key填`-//OpenSymphony Group//XWork Validator Config 1.0//EN`，Alternative web address填`http://struts.apache.org/dtds/xwork-validator-1.0.dtd`。（本地dtd不存在时回去web上去找dtd）

4、StrutsAction-validation.xml需要修改如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE validators PUBLIC
        "-//OpenSymphony Group//XWork Validator Config 1.0//EN"
        "D:\jar\struts2\xwork-validator-1.0.dtd">
<validators>
<!--需要校验的字段的字段名-->
	<field name="name">
		<field-validator type="requiredstring">
			<!--去空格-->
			<param name="trim">true</param>
			<!--错误提示信息-->
			<message>姓名是必须的</message>
		</field-validator>
	</field>
</validators>
```

### 参考文档
修改空格键"="键不上屏：http://blog.csdn.net/afgasdg/article/details/24247863
比较全面的Eclipse配置详解：http://www.cnblogs.com/decarl/archive/2012/05/15/2502084.html

