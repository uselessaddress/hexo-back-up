title: Strust2综合应用实例
date: 2014-02-20 22:10:24
tags:
- JavaEE
- Struts2
categories: 设计开发
---

### 前言

本实例实现了一个简单的功能：添加学生信息。
我们仍然在Struts2概述中struts2项目的基础上进行。

### 建立数据库
使用MySQL，scott用户，建立数据库，名为XSCJ，其中有一张表XSB，结构如下：

| 项目名 | 列名 | 数据类型 | 可空 | 默认值 | 说明 |
| :----: | :----: | :----- | :----: | :----: | :----- |
| 学号 | XH | 定长字符串型（char6）| × | 无 | 主键 |
| 姓名 | XM | 不定长字符串型（varchar8）| × | 无 |  |
| 性别 | XB | 位型（bit）| × | 无 | 值约束：1/0。1表示男，0表示女 |
| 出生时间 | CSSJ | 日期时间型（datetime） | √ | 无 |  |
| 专业Id | ZY_ID | 整数型（int） | × | 无 |  |
| 总学分 | ZXF | 整数型（int） | √ | 0 | 0<=总学分<160 |
| 备注 | BZ | 不定长字符串型（varchar500） | √ | 无 |  |
| 照片 | ZP | longblob | √ | 无 |  |

```
create database xscj;
use xscj;

create table xsb(
xh char(6) not null primary key,
xm varchar(8) not null,
xb bit not null check(xb=0 or xb=1),
cssj datetime ,
zy_id int,
zxf int default 0 check(0<=zxf<160),
bz varchar(500),
zp longblob
);

```
<!--more-->
### 建立stu.jsp
```
<%@ page language="java" pageEncoding="utf-8"%>
<%@ taglib uri="/struts-tags" prefix="s"%>
<%@ taglib uri="/struts-dojo-tags" prefix="sx" %>
<html>
<head>
	<s:head />
	<sx:head/>
</head>
<body>
	<h3>添加学生信息</h3>
	<s:form action="save" namespace="/" method="post" theme="simple">
		<table>
			<tr>
				<td>学号（六位数）：</td>
				<td><s:textfield name="xs.xh"></s:textfield></td>
			</tr>
			<tr>
				<td>姓名：</td>
				<td><s:textfield name="xs.xm" ></s:textfield></td>
			</tr>
			<tr>
				<td>性别：</td>
				<td><s:radio name="xs.xb" list="#{true:'男',false:'女'}" value="true"></s:radio></td>
			</tr>
			<tr>
				<td width="70">出生时间:</td>
				<td><sx:datetimepicker name="xs.cssj" id="cssj"	displayFormat="yyyy-MM-dd"></sx:datetimepicker></td>
			</tr>
			<tr>
				<td>专业ID：</td>
				<td><s:textfield name="xs.zy_id" label="专业"></s:textfield></td>
			</tr>
			<tr>
				<td>备注：</td>
				<td><s:textarea name="xs.bz" label="备注"></s:textarea></td>
			</tr>
			<tr>
				<td><s:submit value="添加"></s:submit></td>
				<td><s:reset value="重置"></s:reset></td>
			</tr>
		</table>
	</s:form>
</body>
</html>

```
注意，上面的代码使用了`/struts-dojo-tags`标签，需要导入包`struts2-dojo-plugin-*.jar`。
Struts2的标签具有自动排版的功能，如果想要自己排版，可以在form标签中加入theme="simple"，但加入该元素后，标签中的label属性就没用了。


### 建立addsuccess.jsp
```
<%@ page language="java" pageEncoding="utf-8"%>
<html>
<head>
</head>
<body>
    	恭喜你，添加成功！
</body>
</html>

```

### 建立JavaBean
```
package com.voidking.struts2.model;

import java.sql.Date;
public class Xsb {
   	private String xh;
	private String xm;
	private boolean xb;
	private Date cssj;
	private int zy_id;
	private int zxf;
	private String bz;
	private byte[] zp;
	public String getXh() {
		return xh;
	}
	public void setXh(String xh) {
		this.xh = xh;
	}
	public String getXm() {
		return xm;
	}
	public void setXm(String xm) {
		this.xm = xm;
	}
	
	public boolean isXb() {
		return xb;
	}
	public void setXb(boolean xb) {
		this.xb = xb;
	}
	public Date getCssj() {
		return cssj;
	}
	public void setCssj(Date cssj) {
		this.cssj = cssj;
	}
	public int getZy_id() {
		return zy_id;
	}
	public void setZy_id(int zy_id) {
		this.zy_id = zy_id;
	}
	public int getZxf() {
		return zxf;
	}
	public void setZxf(int zxf) {
		this.zxf = zxf;
	}
	public String getBz() {
		return bz;
	}
	public void setBz(String bz) {
		this.bz = bz;
	}
	public byte[] getZp() {
		return zp;
	}
	public void setZp(byte[] zp) {
		this.zp = zp;
	}
		
}


```

### 建立DBConn
```
package com.voidking.struts2.db;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;

import com.voidking.struts2.model.Xsb;

public class DBConn {
	Connection conn;
	PreparedStatement pstmt;
	public DBConn(){
		try{
			Class.forName("com.mysql.jdbc.Driver");
			conn=DriverManager.getConnection("jdbc:mysql://localhost/xscj","scott","tiger");
		}catch(Exception e){
			e.printStackTrace();
		}
	}
	// 添加学生
	public boolean save(Xsb xs){
		try{
			pstmt=conn.prepareStatement("insert into XSB(xh,xm,xb,cssj,zy_id,bz) values(?,?,?,?,?,?)");
			pstmt.setString(1, xs.getXh());
			pstmt.setString(2, xs.getXm());
			pstmt.setBoolean(3, xs.isXb());
			pstmt.setDate(4, xs.getCssj());
			pstmt.setInt(5, xs.getZy_id());
			pstmt.setString(6, xs.getBz());
			pstmt.executeUpdate();
			return true;
		}catch(Exception e){
			e.printStackTrace();
			return false;
		}
	}
}


```
注意，别忘记导入包`mysql-connector-java-*-bin.jar`。

### 建立SaveAction
```
package com.voidking.struts2.action;

import com.opensymphony.xwork2.ActionSupport;
import com.voidking.struts2.db.DBConn;
import com.voidking.struts2.model.Xsb;

public class SaveAction extends ActionSupport{
	private Xsb xs;
	public Xsb getXs() {
		return xs;
	}
	public void setXs(Xsb xs) {
		this.xs=xs;
	}
	public String execute() throws Exception {
		DBConn db=new DBConn();
		Xsb stu=new Xsb();
		stu.setXh(xs.getXh());
		stu.setXm(xs.getXm());
		stu.setXb(xs.isXb());
		stu.setZy_id(xs.getZy_id());
		stu.setCssj(xs.getCssj());
		stu.setBz(xs.getBz());
		if(db.save(stu)){
			return SUCCESS;
		}else
			return ERROR;
	}
}


```

### struts.xml
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
        
        <action name="save" class="com.voidking.struts2.action.SaveAction">
        	<result name="success">/addsuccess.jsp</result>
        	<result name="error">/stu.jsp</result>
        	<interceptor-ref name="defaultStack"></interceptor-ref>
        </action>
    </package>
</struts>

```
### 源代码分享
https://github.com/voidking/struts2.git
包括Struts2概述、Struts2国际化应用、Struts2文件上传、Struts2综合应用实例。

### 小结
性别保存为bit类型数据，搞起来感觉有点麻烦，换成char(2)类型，限定值为“男”或“女”，也许更方便一些。

### 参考文档
《Java EE基础实用教程》，郑阿奇主编

