---
title: 公交查询系统
toc: true
date: 2016-12-12 12:01:04
tags:
- java
- freemarker
- art-template
- 百度地图
categories: 设计开发
---
# 交互流程
1、用户进入首页，看到热门公交线路列表。
2、输入关键词，可以搜索到相关公交线路。
3、单击选择公交线路，进入线路方向选择页面。
4、单击选择线路方向，进入百度地图。
5、在初始化百度地图时，传入线路方向。

<!--more-->

# 技术架构
前端技术：jquery和art-template。
后端技术：servlet、freemarker、jdbc和mysql。

# 详细设计
## 前端
### 响应式
本系统适用于移动端，为了适应不同的屏幕，所以前端使用响应式布局。
```
html{font-size:10px}
@media screen and (min-width:321px) and (max-width:375px){html{font-size:11px}}
@media screen and (min-width:376px) and (max-width:414px){html{font-size:12px}}
@media screen and (min-width:415px) and (max-width:639px){html{font-size:15px}}
@media screen and (min-width:640px) and (max-width:719px){html{font-size:20px}}
@media screen and (min-width:720px) and (max-width:749px){html{font-size:22.5px}}
@media screen and (min-width:750px) and (max-width:799px){html{font-size:23.5px}}
@media screen and (min-width:800px){html{font-size:25px}}

*{
    margin: 0;
    padding: 0;
}
```

### 模板引擎
搜索后，搜索结果要显示在页面上。这里，我们选用art-template模板引擎，把搜索请求得到的数据渲染到页面上。
```
<script id="line-template" type="text/html">
<ul>
    {{each lineList as line}}
    <a href="${basePath}/Direction?busName={{line.busName}}">
        <li>{{line.fullName}}({{line.firstStop}}-{{line.lastStop}})</li>
    </a>
    {{/each}}
</ul>
</script>
```

```
$('#search').click(function(event) {
    var basePath = $('#basePath').val();
    var key = $('#key').val();
    $.ajax({
        url: basePath+'/Search',
        type: 'POST',
        dataType: 'json',
        data: {key: key},
        success: function(data){
            console.log(data);
            var html = template('line-template', data);
            $('.content').html(html);
        },
        error: function(xhr){
            console.log(xhr);
        }
    });
});
```

### 百度地图
最终结果，以百度地图的方式展示，需要用到百度地图api。
```
<script type="text/javascript">
    function  init() {
        window.busName = document.getElementById('busName').value;
        window.firstStop = document.getElementById('firstStop').value;
        // console.log(window.busName);
        // console.log(window.firstStop);
    }

    // 百度地图API功能
    var map = new BMap.Map("l-map");            // 创建Map实例
    map.centerAndZoom(new BMap.Point(125.434025,43.83246), 12);

    var busline = new BMap.BusLineSearch(map,{
        renderOptions:{map:map,panel:"r-result"},
            onGetBusListComplete: function(result){
                var directionArr = result.PA;
                console.log(result);
                if(result) {
                    var line = result.getBusListItem(0);
                    var backLine = result.getBusListItem(1);
                    console.log(line);
                    var regx = /\([\u4E00-\u9FA5\uF900-\uFA2D]*-/;
                    var str = line.name;
                    console.log(str);
                    var rs = str.match(regx)[0];
                    rs = rs.slice(1,-1);
                    console.log(rs);
                    if(rs == window.firstStop){
                        busline.getBusLine(line);
                    }else{
                        busline.getBusLine(backLine);
                    }
                }
            }
    });

    function busSearch(){
        busline.getBusList(window.busName);
        //console.log(busline);
    }

    init();
    setTimeout(function(){
        busSearch();
    },1500);
</script>
```

## 后端
### servlet
鉴于本系统业务简单，所以使用servlet就够用了。
```
package com.voidking.servlet;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/Home")
public class Home extends HttpServlet {

    public Home() {
        super();
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // TODO Auto-generated method stub
        response.getWriter().append("Served at: ").append(request.getContextPath());
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // TODO Auto-generated method stub
        doGet(request, response);
    }

}
```

### freemarker
进入首页时，查询出的数据需要渲染到页面上。这里，我们要选择一个后端模板引擎。不习惯使用jsp，决定使用freemarker。
```
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // TODO Auto-generated method stub
    //freemarker配置  
    Configuration config=new Configuration();
    ServletContext context = request.getServletContext();
    config.setServletContextForTemplateLoading(context, "template");
    
    //加载模板文件  
    Template template=config.getTemplate("home.ftl"); 
    
    //创建数据模型  
    Map<String,Object> map=new HashMap<String,Object>();  
    map.put("basePath", request.getContextPath());
    map.put("lineList", lineList);
    
    response.setCharacterEncoding("utf8");
    PrintWriter out = response.getWriter();
    try {
        // 输出模板到页面上
        template.process(map, out);
        out.flush();
        out.close();
    } catch (TemplateException e) {
        e.printStackTrace();
    }
}
```

### json
目前，前后端传输数据最常用的格式不外乎xml和json，本系统采用json作为数据传输格式。
```
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // TODO Auto-generated method stub
    response.setCharacterEncoding("utf8");
    PrintWriter pw = response.getWriter();
    
    String key = request.getParameter("key");
    ArrayList<Line> lineList = lineService.searchLine(key);
    if(lineList.size() > 0){
        jsonObj = new JSONObject("{'code':'0','ext':'success'}");
        jsonObj.put("lineList", lineList);
    }else{
        jsonObj = new JSONObject("{'code':'1','ext':'error'}");
    }
    
    pw.println(jsonObj);
}

```

### jdbc
数据库连接和查询，使用jdbc。
```
public ArrayList<Line> getLines() {
    // 结果集合
    ArrayList<Line> result = new ArrayList<Line>();
    // Step 1: Claim Connection and Statement
    Connection conn = null;
    Statement stmt = null;
    try {
        // STEP 2: Register JDBC driver
        Class.forName(JDBC_DRIVER);

        // STEP 3: Open a connection
        System.out.println("Connecting to database...");
        conn = DriverManager.getConnection(DB_URL, USER, PASS);

        // STEP 4: Execute a query
        System.out.println("Creating statement...");
        stmt = conn.createStatement();
        String sql;
        sql = "select * from line";
        ResultSet rs = stmt.executeQuery(sql);

        // STEP 5: Extract data from result set
        while (rs.next()) {
            // Retrieve by column name
            int id = rs.getInt("id");
            String busName = rs.getString("bus_name");
            String fullName = rs.getString("full_name");
            String firstStop = rs.getString("first_stop");
            String lastStop = rs.getString("last_stop");

            Line line = new Line(id, busName, fullName, firstStop, lastStop);
            result.add(line);
        }
        // STEP 6: Clean-up environment
        rs.close();
        stmt.close();
        conn.close();
    } catch (SQLException se) {
        // Handle errors for JDBC
        se.printStackTrace();
    } catch (Exception e) {
        // Handle errors for Class.forName
        e.printStackTrace();
    } finally {
        // finally block used to close resources
        try {
            if (stmt != null)
                stmt.close();
        } catch (SQLException se2) {
        } // nothing we can do
        try {
            if (conn != null)
                conn.close();
        } catch (SQLException se) {
            se.printStackTrace();
        } // end finally try
    } // end try
    System.out.println("Goodbye!");
    return result;
}
```


## 数据库
### 数据搜集
长春公交有近200条公交线路，这里，我们从8684网站上搜集了常用的11条线路用来做测试。

### 表设计
```
CREATE TABLE `line` (
  `id` int(4) NOT NULL AUTO_INCREMENT,
  `bus_name` varchar(32) NOT NULL,
  `full_name` tinytext NOT NULL,
  `first_stop` varchar(32) DEFAULT NULL,
  `last_stop` varchar(32) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

# 效果演示
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/bus-search/result.gif)

# 源码分享
https://github.com/voidking/bus-search

# 后记
为什么不做实时公交查询？因为拿不到数据。百度地图没有提供实时公交API，也拿不到公交公司的数据。

# 书签
Introducing JSON
http://www.json.org/

JSON入门之二：org.json的基本用法
http://blog.csdn.net/jediael_lu/article/details/25779087

JSON解析工具-org.json使用教程
http://www.open-open.com/lib/view/open1381566882614.html

Amaze UI 模板中心
http://tpl.amazeui.org/

（淘宝无限适配）手机端rem布局详解
http://www.cnblogs.com/well-nice/p/5509589.html

长春公交查询
http://changchun.8684.cn/

百度地图JavaScript 开源库
http://lbsyun.baidu.com/index.php?title=open/library

百度地图API示例
http://lbsyun.baidu.com/jsdemo.htm#a1_2
