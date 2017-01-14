title: Strust2文件上传
date: 2014-02-20 21:10:24
tags:
- Strust2
- JavaEE
categories: 设计开发
---

## Struts2文件上传

### 上传单个文件
Struts2中，提供了一个很容易操作的文件上传组件。
用Struts2上传单个文件的功能非常容易实现，只要使用普通的Action即可。但为了获得一些文件上传的信息，如上传文件名等，需要按照一定规则来为Action类增加一些getter和setter方法。
Struts2的文件上传默认使用的是Jakarta的Common-FileUpload文件上传框架。因此需要在Web应用中增加两个Jar包，即common-io-\*.jar和common-fileupload-\*.jar。

我们接着在struts2概述中的struts2项目中开发文件上传实例。

#### 指定文件夹
在D盘下新建upload文件夹，上传的文件放到这个目录下。
<!--more-->
#### upload.jsp
在WebContent中新建upload.jsp，代码如下：
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="/struts-tags" prefix="s"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>文件上传</title>
</head>
<body>
	<s:form action="upload.action" method="post" enctype="multipart/form-data">
		<s:file name="upload" label="上传的文件"></s:file>
		<s:submit value="上传"></s:submit>
	</s:form>

</body>
</html>

```
#### UploadAction
```
package com.voidking.struts2.action;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.io.OutputStream;

import com.opensymphony.xwork2.ActionSupport;

public class UploadAction extends ActionSupport{
	private File upload;
	private String uploadFileName;
	
	public String getUploadFileName() {
		return uploadFileName;
	}

	public void setUploadFileName(String uploadFileName) {
		this.uploadFileName = uploadFileName;
	}

	public File getUpload()
	{
		return upload;
	}
	
	public void setUpload(File upload)
	{
		this.upload=upload;
	}
	
	public String execute() throws Exception{
		InputStream is=new FileInputStream(getUpload());
		OutputStream os = new FileOutputStream("d:\\upload\\"+uploadFileName);
		byte buffer[] = new byte[1024];
		int count= 0;
		while((count=is.read(buffer))>0)
		{
			os.write(buffer,0,count);
		}
		os.close();
		is.close();
		return SUCCESS;
	}
	
}

```
上传的文件经过Action处理后，会被写到指定的路径下。其实也可以把上传的文件写入数据库中，之后的例子会介绍如何把照片写入到数据库。
需要注意的是，Struts2上传文件的默认大小限制为2MB，如果需要修改默认大小，只需要在Struts2的struts.properties文件中修改struts.multipart.maxSize。如struts.mulitipart.maxSize=1024表示上传文件的总大小不能超过1KB。


#### struts.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <package name="default" namespace="/" extends="struts-default">
    
    	<interceptors>
    		<interceptor name="myInterceptor" class="com.voidking.struts2.tool.MyInterceptor"></interceptor>
    	</interceptors>
    	<default-interceptor-ref name=""></default-interceptor-ref>
    	
        <default-action-ref name="index" />
        <action name="struts" class="com.voidking.struts2.action.StrutsAction">
            <result name="success">/welcome.jsp</result>
            <result name="error">/hello.jsp</result>
            <result name="input">/hello.jsp</result>
            
            <!-- 拦截配置在result后面 -->
            <!-- 使用系统默认拦截器栈 -->
            <interceptor-ref name="defaultStack"></interceptor-ref>
            <!-- 配置拦截器 -->
            <interceptor-ref name="myInterceptor"></interceptor-ref>
            
        </action>
        
        <action name="upload" class="com.voidking.struts2.action.UploadAction">
        	<result name="success">/uploadsuccess.jsp</result>
        </action>
    </package>
</struts>

```

#### uploadsuccess.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>上传成功</title>
</head>
<body>
	恭喜你，上传成功！
</body>
</html>

```

#### 404错误
部署项目，启动Tomcat，访问：http://localhost:8080/struts2/upload.jsp ，提示 HTTP Status 404 - /struts2/upload.jsp。访问主页：http://localhost:8080/struts2/ ，同样提示404错误！

啊勒，这是肿么回事？想来想去，引起错误的原因，可能是昨天关闭eclipse的时候，没有先关闭Tomcat服务器，引起了一些配置错误。

查看tomcat/conf文件夹下的配置文件，server.xml和tomcat-users.xml，感觉没啥问题。

访问：http://localhost:8080 ，使用manager-gui账户登录tomcat，Manager App，可以看到，struts2项目的Running属性的值为false。奥~这就是问题所在了！点击start按钮，啊勒，无效！

删除webapps下的struts2工程，重新发布，运行tomcat，还是404错误！

再次点击start按钮，同时观察Console下面的信息：
```
SEVERE: Exception starting filter struts2
Unable to load configuration. - package - file:/D:/Server/tomcat/webapps/struts2/WEB-INF/classes/struts.xml:7:67
......
SEVERE: Error filterStart
```

经过查找资料，确定了是struts.xml的配置问题。首先修改struts.xm如下：
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <package name="default" namespace="/" extends="struts-default">
    
    	<interceptors>
    		<interceptor name="myInterceptor" class="com.voidking.struts2.tool.MyInterceptor"></interceptor>
    	</interceptors>
    	<default-interceptor-ref name=""></default-interceptor-ref>
    	
        <default-action-ref name="index" />
        <action name="struts" class="com.voidking.struts2.action.StrutsAction">
            <result name="success">/welcome.jsp</result>
            <result name="error">/hello.jsp</result>
            <result name="input">/hello.jsp</result>
            
            <!-- 拦截配置在result后面 -->
            <!-- 使用系统默认拦截器栈 -->
            <interceptor-ref name="defaultStack"></interceptor-ref>
            <!-- 配置拦截器 -->
            <interceptor-ref name="myInterceptor"></interceptor-ref>
            
        </action>
    </package>
</struts>

```
启动Tomcat，项目果然可以访问了！那么，为什么在package中多加了一个action就会出错呢？很多教程中，明明可以多个action写在一个package里啊！再看这个package，有何特殊之处？配置了拦截器！那么，接下来的action是否必须配置拦截器？修改试试。
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
    <package name="default" namespace="/" extends="struts-default">
    
    	<interceptors>
    		<interceptor name="myInterceptor" class="com.voidking.struts2.tool.MyInterceptor"></interceptor>
    	</interceptors>
    	<default-interceptor-ref name=""></default-interceptor-ref>
    	
        <default-action-ref name="index" />
        <action name="struts" class="com.voidking.struts2.action.StrutsAction">
            <result name="success">/welcome.jsp</result>
            <result name="error">/hello.jsp</result>
            <result name="input">/hello.jsp</result>
            
            <!-- 拦截配置在result后面 -->
            <!-- 使用系统默认拦截器栈 -->
            <interceptor-ref name="defaultStack"></interceptor-ref>
            <!-- 配置拦截器 -->
            <interceptor-ref name="myInterceptor"></interceptor-ref>
            
        </action>
        
        <action name="upload" class="com.voidking.struts2.action.UploadAction">
        	<result name="success">/uploadsuccess.jsp</result>
        	<interceptor-ref name="defaultStack"></interceptor-ref>
        </action>
    </package>
</struts>

```
启动Tomcat，项目可以正常访问。综上，可以得出一个结论：package包中如果定义了拦截器，那么，接下来的action中必须配置拦截器，否则会引起项目无法启动！

#### 警告
我们的项目已经可以正常运行，上传功能也正常。但是，当我们访问：http://localhost:8080/struts2/upload.jsp ，会报出如下警告：
```
'upload.action' in namespace: ''. Form action defaulting to 'action' attribute's literal value.
```
经过查找资料，原来这是因为没有正确使用tag，upload.jsp修改如下：
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="/struts-tags" prefix="s"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>文件上传</title>
</head>
<body>
	<s:form action="upload" namespace="/" method="post" enctype="multipart/form-data">
		<s:file name="upload" label="上传的文件"></s:file>
		<s:submit value="上传"></s:submit>
	</s:form>

</body>
</html>
```
再次访问，已经没有警告了。但是还是有提示信息：
```
At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.

```
对于有洁癖的同学可以参考下面的参考文档去除这个信息，小编这里不再赘述。


### 多文件上传
多文件上传和单文件上传类似，只需要改动几个地方即可。

#### upload.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="/struts-tags" prefix="s"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>文件上传</title>
</head>
<body>
	<s:form action="upload" namespace="/" method="post" enctype="multipart/form-data">
		<s:file name="upload" label="上传的文件"></s:file>
		<s:submit value="上传"></s:submit>
	</s:form>
	<hr/>
	<s:form action="upload2" namespace="/" method="post" enctype="multipart/form-data">
		<s:file name="upload2" label="上传的文件"></s:file>
		<s:file name="upload2" label="上传的文件"></s:file>
		<s:file name="upload2" label="上传的文件"></s:file>
		<s:submit value="批量上传"></s:submit>
	</s:form>
</body>
</html>
```

#### Upload2Action
```
package com.voidking.struts2.action;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.List;

import com.opensymphony.xwork2.ActionSupport;

public class Upload2Action extends ActionSupport{
	private List<File> upload2;
	private List<String> upload2FileName;
	
	
	
	public List<File> getUpload2() {
		return upload2;
	}



	public void setUpload2(List<File> upload2) {
		this.upload2 = upload2;
	}


	public List<String> getUpload2FileName() {
		return upload2FileName;
	}



	public void setUpload2FileName(List<String> upload2FileName) {
		this.upload2FileName = upload2FileName;
	}



	public String execute() throws Exception{
		
		if(upload2!=null || upload2FileName!=null)
		{
			for(int i=0;i<upload2.size();i++)
			{
				InputStream is = new FileInputStream(upload2.get(i));
				OutputStream os=new FileOutputStream("d:\\upload\\"+getUpload2FileName().get(i));
				byte buffer[] = new byte[1024];
				int count=0;
				while((count=is.read(buffer))>0)
				{
					os.write(buffer,0,count);
				}
				os.close();
				is.close();
				
				return SUCCESS;
			}
		}
		
		return ERROR;
	}
	
}

```
这里需要注意的是属性命名问题，如果`private List<File> upload2;`，那么必须`private List<String> upload2FileName;`，否则无法获取到文件名。

#### struts.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>
	<constant name="struts.multipart.saveDir" value="/tmp"/>
    <package name="default"  extends="struts-default">
    
    	<interceptors>
    		<interceptor name="myInterceptor" class="com.voidking.struts2.tool.MyInterceptor"></interceptor>
    	</interceptors>
    	<default-interceptor-ref name=""></default-interceptor-ref>
    	
        <default-action-ref name="index" />
        <action name="struts" class="com.voidking.struts2.action.StrutsAction">
            <result name="success">/welcome.jsp</result>
            <result name="error">/hello.jsp</result>
            <result name="input">/hello.jsp</result>
            
            <!-- 拦截配置在result后面 -->
            <!-- 使用系统默认拦截器栈 -->
            <interceptor-ref name="defaultStack"></interceptor-ref>
            <!-- 配置拦截器 -->
            <interceptor-ref name="myInterceptor"></interceptor-ref>
            
        </action>
        
        <action name="upload" class="com.voidking.struts2.action.UploadAction">
        	<result name="success">/uploadsuccess.jsp</result>
        	<interceptor-ref name="defaultStack"></interceptor-ref>
        </action>
        
        <action name="upload2" class="com.voidking.struts2.action.Upload2Action">
        	<result name="success">/uploadsuccess.jsp</result>
        	<interceptor-ref name="defaultStack"></interceptor-ref>
        </action>
    </package>
</struts>

```
其中，`<constant name="struts.multipart.saveDir" value="/tmp"/>`最好保留，不然会有提示信息:
`INFO: Unable to find 'struts.multipart.saveDir' property setting.`。另一个解决办法是在struts.properties加入：
`struts.multipart.saveDir = /tmp`。


### 参考文档
《Java EE基础实用教程》，郑阿奇主编

------
Form action defaulting to 'action' attribute's literal value：
http://www.blogjava.net/parable-myth/archive/2010/10/28/336387.html

------
解决Tomcat7“At least one JAR was scanned for TLDs yet contained no TLDs”问题：
http://mov-webhobo.iteye.com/blog/1939655
http://love-love-l.blog.163.com/blog/static/2107830420131159580327/

------
