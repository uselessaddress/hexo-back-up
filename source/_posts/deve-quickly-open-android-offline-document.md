title: 加快Android离线文档的访问速度
date: 2015-05-20 08:17:21
tags: [Android]
categories: 设计开发
toc: true
---


### 前言
学习编程三年，已经确信，学习一门技术最好的资料就是官方文档。如今，拖沓了很久的Android学习计划，终于启动。学习资料：Android官方文档；学习方法：写demo写博客（总结）。

### 阅读途径

#### 官网
http://developer.android.com/index.html
不幸的是，这个网站的国内访问速度，慢到令人发指。很多时候，根本打不开。当然，如果你喜欢翻墙玩，就另当别论。

#### 国内镜像站
不得不说，咱们国家有很多雷锋。国外的网站不好访问，那就在国内搭建一个镜像站，方便大家访问。这里小编推荐几个国内镜像站供大家使用：
1、踏得网
http://wear.techbrood.com/training/index.html

2、Android中文API
http://www.android-doc.com
http://www.android-doc.com/reference/packages.html
http://www.android-doc.com/guide/components/activities.html

3、Android官方培训课程中文版(v0.9.2)
http://hukai.me/android-training-course-in-chinese/index.html

<!--more-->
#### 离线文档
打开adt中的SDK Manager，安装Documentation for Android SDK。安装好之后，在adt/sdk/docs目录下，就是官方文档了。
当然，我们也可以直接下载别人下载打包好的官方文档，速度会快很多。

这时候问题来了，docs里的很多文件，会加载谷歌的字体和js。这样，严重拖慢了打开速度。

### 解决办法
#### 修改index.html
打开index.html，然后注释掉stylesheet和js两个地方：
```
<!--<link rel="stylesheet" href="http://fonts.googleapis.com/css?family=Roboto:regular,medium,thin,italic,mediumitalic,bold" title="roboto">-->
<!--<script src="http://www.google.com/jsapi" type="text/javascript"></script>-->
```
#### 修改本机hosts文件

C:\Windows\System32\drivers\etc\hosts
增加如下部分：
```
127.0.0.1 fonts.googleapis.com
127.0.0.1 www.google.com
```
#### Java实现批量注释
```
/*
 * 去掉Android文档中需要联网的javascript代码
 */
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class FormatDoc {
    public static int j=1;
    /**
     * @param args
     */
    public static void main(String[] args) {
        
        File file = new File("D:/android/android-sdk-windows/docs/");
        searchDirectory(file, 0);
        System.out.println("OVER");
    }

    public static void searchDirectory(File f, int depth) {
        if (!f.isDirectory()) {
            String fileName = f.getName();
            if (fileName.matches(".*.{1}html")) {
                String src= "<(link rel)[=]\"(stylesheet)\"\n(href)[=]\"(http)://(fonts.googleapis.com/css)[?](family)[=](Roboto)[:](regular,medium,thin,italic,mediumitalic,bold)\"( title)[=]\"roboto\">";
                String src1 = "<script src=\"http://www.google.com/jsapi\" type=\"text/javascript\"></script>";
                String dst = "";
                //如果是html文件则注释掉其中的特定javascript代码
                annotation(f, src, dst);
                annotation(f, src1, dst);
            }
        } else {
            File[] fs = f.listFiles();
            depth++;
            for (int i = 0; i < fs.length; ++i) {
                File file = fs[i];
                searchDirectory(file, depth);
            }
        }
    }

    /*
     * f 将要修改其中特定内容的文件 
     * src 将被替换的内容 
     * dst 将被替换层的内容
     */
    public static void annotation(File f, String src, String dst) {
        String content = FormatDoc.read(f);
        content = content.replaceFirst(src, dst);
        int ll=content.lastIndexOf(src);
        System.out.println(ll);
        FormatDoc.write(content, f);
        System.out.println(j++);
        return;

    }

    public static String read(File src) {
        StringBuffer res = new StringBuffer();
        String line = null;
        try {
            BufferedReader reader = new BufferedReader(new FileReader(src));
            int i=0;
            while ((line = reader.readLine()) != null) {
                if (i!=0) {
                    res.append('\n');
                }
                res.append(line);
                i++;
            }
            reader.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return res.toString();
    }

    public static boolean write(String cont, File dist) {
        try {
            BufferedWriter writer = new BufferedWriter(new FileWriter(dist));
            writer.write(cont);
            writer.flush();
            writer.close();
            return true;
        } catch (IOException e) {
            e.printStackTrace();
            return false;
        }
    }
}
```

#### C++实现批量注释
详见参考文档：优化Android离线文档的访问速度。
velable兄很牛逼，感谢他分享给我们的代码和文档！小编存入了网盘一份，方便大家下载。
源代码：http://yunpan.cn/cj7VgRg9tUS6e  访问密码 edb4
文档：http://yunpan.cn/cj7Vsaavmey62  访问密码 4e38


### 参考文档
Android帮助文档本地打开慢的解决方案
http://blog.csdn.net/wc0077/article/details/39669885

-------
优化Android离线文档的访问速度 
http://git.oschina.net/velable/OptAndroidDocs

-------
Android离线文档，急速访问+搜索功能正常。
http://git.oschina.net/velable/Android_Offline_Docs

-------