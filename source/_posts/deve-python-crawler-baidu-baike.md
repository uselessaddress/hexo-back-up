---
title: Python抓取百度百科数据
toc: true
date: 2017-01-22 09:10:00
tags:
- python
- 爬虫
categories: 设计开发
---
# 前言
本文整理自慕课网[《Python开发简单爬虫》](http://www.imooc.com/learn/563)，将会记录爬取百度百科“python”词条相关页面的整个过程。

<!--more-->

# 抓取策略
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/crawler-baidu-baike/analysis.jpg)
确定目标：确定抓取哪个网站的哪些页面的哪部分数据。本实例抓取百度百科python词条页面以及python相关词条页面的标题和简介。
分析目标：分析要抓取的url的格式，限定抓取范围。分析要抓取的数据的格式，本实例中就要分析标题和简介这两个数据所在的标签的格式。分析要抓取的页面编码的格式，在网页解析器部分，要指定网页编码，然后才能进行正确的解析。
编写代码：在网页解析器部分，要使用到分析目标得到的结果。
执行爬虫：进行数据抓取。

# 分析目标
1、url格式
进入百度百科python词条页面，页面中相关词条的链接比较统一，大都是`/view/xxx.htm`。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/crawler-baidu-baike/href.jpg)

2、数据格式
标题位于类lemmaWgt-lemmaTitle-title下的h1子标签，简介位于类lemma-summary下。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/crawler-baidu-baike/title.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/crawler-baidu-baike/summary.jpg)

3、编码格式
查看页面编码格式，为utf-8。
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/crawler-baidu-baike/charset.jpg)

经过以上分析，得到结果如下：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/crawler-baidu-baike/result.jpg)

# 代码编写
## 项目结构
在sublime下，新建文件夹baike-spider，作为项目根目录。
新建spider_main.py，作为爬虫总调度程序。
新建url_manger.py，作为url管理器。
新建html_downloader.py，作为html下载器。
新建html_parser.py，作为html解析器。
新建html_outputer.py，作为写出数据的工具。
最终项目结构如下图：
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/crawler-baidu-baike/structure.jpg)

## spider_main.py
```
# coding:utf-8
import url_manager, html_downloader, html_parser, html_outputer

class SpiderMain(object):
    def __init__(self):
        self.urls = url_manager.UrlManager()
        self.downloader = html_downloader.HtmlDownloader()
        self.parser = html_parser.HtmlParser()
        self.outputer = html_outputer.HtmlOutputer()

    def craw(self, root_url):
        count = 1
        self.urls.add_new_url(root_url)
        while self.urls.has_new_url():
            try:
                new_url = self.urls.get_new_url()
                print('craw %d : %s' % (count, new_url))
                html_cont = self.downloader.download(new_url)
                new_urls, new_data = self.parser.parse(new_url, html_cont)
                self.urls.add_new_urls(new_urls)
                self.outputer.collect_data(new_data)

                if count == 10:
                    break

                count = count + 1
            except:
                print('craw failed')

        self.outputer.output_html()


if __name__=='__main__':
    root_url = 'http://baike.baidu.com/view/21087.htm'
    obj_spider = SpiderMain()
    obj_spider.craw(root_url)
```

## url_manger.py
```
# coding:utf-8
class UrlManager(object):
    def __init__(self):
        self.new_urls = set()
        self.old_urls = set()

    def add_new_url(self, url):
        if url is None:
            return
        if url not in self.new_urls and url not in self.old_urls:
            self.new_urls.add(url)

    def add_new_urls(self, urls):
        if urls is None or len(urls) == 0:
            return
        for url in urls:
            self.add_new_url(url)

    def has_new_url(self):
        return len(self.new_urls) != 0

    def get_new_url(self):
        new_url = self.new_urls.pop()
        self.old_urls.add(new_url)
        return new_url

```

## html_downloader.py
```
# coding:utf-8
import urllib.request

class HtmlDownloader(object):
    def download(self, url):
        if url is None:
            return None
        response = urllib.request.urlopen(url)
        if response.getcode() != 200:
            return None
        return response.read()
```

## html_parser.py
```
# coding:utf-8
from bs4 import BeautifulSoup
import re
from urllib.parse import urljoin

class HtmlParser(object):
    def _get_new_urls(self, page_url, soup):
        new_urls = set()
        # /view/123.htm
        links = soup.find_all('a', href=re.compile(r'/view/\d+\.htm'))
        for link in links:
            new_url = link['href']
            new_full_url = urljoin(page_url, new_url)
            # print(new_full_url)
            new_urls.add(new_full_url)
        #print(new_urls)
        return new_urls

    def _get_new_data(self, page_url, soup):
        res_data = {}
        # url
        res_data['url'] = page_url
        # <dd class="lemmaWgt-lemmaTitle-title"> <h1>Python</h1>
        title_node = soup.find('dd', class_='lemmaWgt-lemmaTitle-title').find('h1')
        res_data['title'] = title_node.get_text()
        # <div class="lemma-summary" label-module="lemmaSummary">
        summary_node = soup.find('div', class_='lemma-summary')
        res_data['summary'] = summary_node.get_text()
        # print(res_data)
        return res_data

    def parse(self, page_url, html_cont):
        if page_url is None or html_cont is None:
            return
        soup = BeautifulSoup(html_cont, 'html.parser')
        # print(soup.prettify())
        new_urls = self._get_new_urls(page_url, soup)
        new_data = self._get_new_data(page_url, soup)
        # print('mark')
        return new_urls, new_data
```

## html_outputer.py
```
# coding:utf-8
class HtmlOutputer(object):
    def __init__(self):
        self.datas = []

    def collect_data(self, data):
        if data is None:
            return
        self.datas.append(data)

    def output_html(self):
        fout = open('output.html','w', encoding='utf-8')

        fout.write('<html>')
        fout.write('<body>')
        fout.write('<table>')

        for data in self.datas:
            fout.write('<tr>')
            fout.write('<td>%s</td>' % data['url'])
            fout.write('<td>%s</td>' % data['title'])
            fout.write('<td>%s</td>' % data['summary'])
            fout.write('</tr>')

        fout.write('</table>')
        fout.write('</body>')
        fout.write('</html>')

        fout.close()
```

## 运行
在命令行下，执行`python spider_main.py`。

## 编码问题
问题描述：UnicodeEncodeError: 'gbk' codec can't encode character '\xa0' in position ... 

使用Python写文件的时候，或者将网络数据流写入到本地文件的时候，大部分情况下会遇到这个问题。网络上有很多类似的文章讲述如何解决这个问题，但是无非就是encode，decode相关的，这是导致该问题出现的真正原因吗？不是的。很多时候，我们使用了decode和encode，试遍了各种编码，utf8，utf-8，gbk，gb2312等等，该有的编码都试遍了，可是仍然出现该错误，令人崩溃。

在windows下面编写python脚本，编码问题很严重。将网络数据流写入文件时，我们会遇到几个编码：
1、#encoding='XXX' 
这里(也就是python文件第一行的内容)的编码是指该python脚本文件本身的编码，无关紧要。只要XXX和文件本身的编码相同就行了。 
比如notepad++"格式"菜单里面里可以设置各种编码，这时需要保证该菜单里设置的编码和encoding XXX相同就行了，不同的话会报错。

2、网络数据流的编码
比如获取网页，那么网络数据流的编码就是网页的编码。需要使用decode解码成unicode编码。

3、目标文件的编码 
将网络数据流写入到新文件，写文件代码如下：
```
fout = open('output.html','w')
fout.write(str)
```
在windows下面，新文件的默认编码是gbk，python解释器会用gbk编码去解析我们的网络数据流str，然而str是decode过的unicode编码，这样的话就会导致解析不了，出现上述问题。 解决的办法是改变目标文件的编码：
```
fout = open('output.html','w', encoding='utf-8')
```

## 运行结果
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/crawler-baidu-baike/result1.jpg)
![](http://7oxjrx.com1.z0.glb.clouddn.com//imgs/crawler-baidu-baike/result2.jpg)

# 源码分享
https://github.com/voidking/baike-spider

# 书签
Python开发简单爬虫
http://www.imooc.com/learn/563

The Python Standard Library
https://docs.python.org/3/library/index.html

Beautiful Soup 4.2.0 文档
https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html

Python词条
http://baike.baidu.com/view/21087.htm
http://baike.baidu.com/item/Python

Python3.x爬虫教程：爬网页、爬图片、自动登录
http://www.2cto.com/kf/201507/417660.html

使用python3进行优雅的爬虫（一）爬取图片
http://www.jianshu.com/p/696922f268df

Python UnicodeEncodeError: 'gbk' codec can't encode character 解决方法
http://www.jb51.net/article/64816.htm

Scrapy documentation
https://doc.scrapy.org/en/latest/


