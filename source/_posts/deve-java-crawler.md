---
title: Java网页爬虫
toc: true
date: 2016-11-26 12:04:06
tags:
- java
- crawler
categories: 设计开发
---
# 功能进阶
1、Java网页爬虫，最基础的功能，是能爬取某个页面的html源码。
2、图形化界面。
3、爬取某个页面的html源码，以及页面需要的静态资源（图片、css和js）。
4、爬取某个页面的html源码，以及页面中的链接指向的页面的html源码，并且不断地延伸爬取。

整个开发过程，需要用到网络编程、正则表达式、I/O流、图形界面编程、事件监听、多线程等。为了简化开发，还需要用到一些外部jar包，比如jsoup。

<!--more-->

# 模块划分
1、获取页面模块：获取页面文档，以及页面文档的字符串。
2、获取页面链接模块：获取页面中存在的各种链接，包括a标签、img标签、css链接、js链接等。
3、获取静态文件模块：静态文件分为两种，一种是字符串类型，一种是字节类型。
4、保存文件模块：保存页面文档和静态文件。
5、界面模块：包括界面设计，事件监听处理。

# 核心代码摘要
## 获取页面模块
```
public class Page {
    Document doc = null;
    public Page(String url) {
        try {
            doc = Jsoup.connect(url).timeout(5000).userAgent("Mozilla").get();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    public String getHtml(){
        return doc.html();
    }
    
    public Document getDoc(){
        return doc;
    }
}
```

## 获取页面链接模块
```
public class UrlList {
    public Document doc = null;
    public UrlList(String url) {
        try {
            doc = Jsoup.connect(url).timeout(5000).userAgent("Mozilla").get();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    public ArrayList<Element> getLinkList(){
        Elements links = doc.select("link[href]");
        ArrayList<Element> resultList = new ArrayList<Element>();
        for(Element link : links){
            resultList.add(link);
        }
        return resultList;
    }
    
    public ArrayList<Element> getCssList(){
        Elements links = doc.select("link[href]");
        ArrayList<Element> resultList = new ArrayList<Element>();
        for(Element link : links){
            if("stylesheet".equals(link.attr("rel"))){
                resultList.add(link);               
            }
        }
        return resultList;
    }
    
    public ArrayList<Element> getAList(){
        Elements links = doc.select("a[href]");
        ArrayList<Element> resultList = new ArrayList<Element>();
        for(Element link : links){
            resultList.add(link);
        }
        return resultList;
    }
    
    public ArrayList<Element> getImgList(){
        Elements links = doc.select("img[src]");
        ArrayList<Element> resultList = new ArrayList<Element>();
        for(Element link : links){
            resultList.add(link);
        }
        return resultList;
    }
    
    public ArrayList<Element> getJsList(){
        Elements links = doc.select("script[src]");
        ArrayList<Element> resultList = new ArrayList<Element>();
        for(Element link : links){
            resultList.add(link);
        }
        return resultList;
    }
} 
```

## 获取静态文件
```
public class Source {
    public String getString(String url) { // 定义一个字符串用来存储网页内容
        String result = "";
        // 定义一个缓冲字符输入流
        BufferedReader in = null;
        try {
            // 将string转成url对象
            URL realUrl = new URL(url);
            // 初始化一个链接到那个url的连接
            URLConnection connection = realUrl.openConnection();
            // 开始实际的连接
            connection.connect();
            // 初始化 BufferedReader输入流来读取URL的响应
            in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            // 用来临时存储抓取到的每一行的数据
            String line;
            while ((line = in.readLine()) != null) {
                // 遍历抓取到的每一行并将其存储到result里面
                result += line;
            }
        } catch (Exception e) {
            System.out.println("发送GET请求出现异常！" + e);
            e.printStackTrace();
        } finally {
            try {
                if (in != null) {
                    in.close();
                }
            } catch (Exception e2) {
                e2.printStackTrace();
            }
        }
        return result;
    }

}
```

## 保存文件模块
```
public class SaveFile {

    public void saveFile(String path, String htmlStr) {

        File file = new File(path + "\\爬取的文件\\index.html");
        if (!file.getParentFile().exists()) {
            file.getParentFile().mkdirs();
        }

        // 字节输出流
        FileOutputStream fos = null;
        try {
            fos = new FileOutputStream(file);
            OutputStreamWriter writer = new OutputStreamWriter(fos, "UTF-8");
            writer.write(htmlStr);
            writer.close();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                fos.close();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }

    }

    public void saveCss(String path, ArrayList<Element> cssList) {
        for (int i = 0; i < cssList.size(); i++) {
            if (!cssList.get(i).attr("abs:href").equals(cssList.get(i).attr("href"))) {
                Source source = new Source();
                String css = source.getString(cssList.get(i).attr("abs:href"));

                String paths[] = cssList.get(i).attr("href").split("\\?");
                File file = new File(path + "\\爬取的文件\\" + paths[0]);
                if (!file.getParentFile().exists()) {
                    file.getParentFile().mkdirs();
                }

                // 字节输出流
                FileOutputStream fos = null;
                try {
                    fos = new FileOutputStream(file);
                    OutputStreamWriter writer = new OutputStreamWriter(fos, "UTF-8");
                    writer.write(css);
                    writer.close();
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    try {
                        fos.close();
                    } catch (IOException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
            }
        }

    }

    public void saveImg(String path, ArrayList<Element> imgList) {
        for (int i = 0; i < imgList.size(); i++) {
            if (!imgList.get(i).attr("abs:src").equals(imgList.get(i).attr("src"))) {
                String paths[] = imgList.get(i).attr("src").split("\\?");
                File file = new File(path + "\\爬取的文件\\" + paths[0]);
                if (!file.getParentFile().exists()) {
                    file.getParentFile().mkdirs();
                }
                try {
                    URL uri = new URL(imgList.get(i).attr("abs:src"));  
                    InputStream in = uri.openStream();  
                    FileOutputStream fo = new FileOutputStream(file);  
                    byte[] buf = new byte[1024];  
                    int length = 0;  
                    while ((length = in.read(buf, 0, buf.length)) != -1) {  
                        fo.write(buf, 0, length);  
                    }  
                    in.close();  
                    fo.close();  
                } catch (Exception e) {  
                    e.printStackTrace();  
                }  
                
            }
        }

    }
    
    public void saveJs(String path, ArrayList<Element> jsList){
        for (int i = 0; i < jsList.size(); i++) {
            if (!jsList.get(i).attr("abs:src").equals(jsList.get(i).attr("src"))) {
                Source source = new Source();
                String css = source.getString(jsList.get(i).attr("abs:src"));

                String paths[] = jsList.get(i).attr("src").split("\\?");
                File file = new File(path + "\\爬取的文件\\" + paths[0]);
                if (!file.getParentFile().exists()) {
                    file.getParentFile().mkdirs();
                }

                // 字节输出流
                FileOutputStream fos = null;
                try {
                    fos = new FileOutputStream(file);
                    OutputStreamWriter writer = new OutputStreamWriter(fos, "UTF-8");
                    writer.write(css);
                    writer.close();
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    try {
                        fos.close();
                    } catch (IOException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
            }
        }
    }
}
```

## 界面模块
代码略，来个图说明设计。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/java-crawler/result.gif)

# 后记
未填的坑：
1、最终设计做到了功能3，功能4未做。
2、爬取文件的相对位置需要另做处理。

# 源码分享
https://github.com/voidking/java-crawler

# 书签
如何用Java写一个爬虫？
https://www.zhihu.com/question/30626103

零基础写Java知乎爬虫之先拿百度首页练练手
http://www.jb51.net/article/57193.htm

网页爬虫的设计与实现（Java版）
http://www.aiuxian.com/article/p-2279197.html

网页爬虫系统的设计
http://www.tuicool.com/articles/7JVzIza

专栏：使用JSOUP实现网络爬虫
http://blog.csdn.net/column/details/jsoup.html

jsoup Cookbook(中文版)
http://www.open-open.com/jsoup/