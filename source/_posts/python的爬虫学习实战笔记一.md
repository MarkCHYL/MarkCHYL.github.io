---
layout: post
title: python的爬虫学习实战笔记一
date: 2018-12-17 10:58:01
toc: true
reward: true
tags: [Python,爬虫]
categories: [Python]
---
### 前言：
#### Python3的爬虫基础学习，编写URL管理器、网页下载器、网页分析器、数据输出器、如何连接到MySQL。网上能搜到许多的库，在这我只是开始我的基础学起，瞎猫死猫能运行就可以。
---
<!--more-->
#### 编写的环境：
Mac电脑、MySQL数据库、pymysql库、urllib库、bs4库、Python3、PyCharm编辑器

[学习网址](http://www.runoob.com/python3/python3-dictionary.html)

### 定义：
网络爬虫（Web Spider），又被称为网页蜘蛛，是一种按照一定的规则，自动地抓取网站信息的程序或者脚本。

### 简介：
网络蜘蛛是一个很形象的名字。如果把互联网比喻成一个蜘蛛网，那么Spider就是在网上爬来爬去的蜘蛛。网络蜘蛛是通过网页的链接地址来寻找网页，从 网站某一个页面开始，读取网页的内容，找到在网页中的其它链接地址，然后通过这些链接地址寻找下一个网页，这样一直循环下去，直到把这个网站所有的网页都抓取完为止

### 爬虫流程：
```
①先由urllib的request打开Url得到网页html文档
——②浏览器打开网页源代码分析元素节点
——③通过BeautifulSoup或则正则表达式提取想要的数据
——④存储数据到本地磁盘或数据库（抓取，分析，存储）
```
## 上代码
爬虫程序入口：Spider_Main
爬虫目标URL：http://www.hbooker.com/book/10007188

```
from hbooker_spider import html_downloader
from hbooker_spider import html_outputer
from hbooker_spider import html_parser
from hbooker_spider import url_manager


class SpiderMain(object):
    def __init__(self):
        # 初始化 URL 管理器
        self.urls = url_manager.UrlManager()
        # 初始化 html 下载器
        self.downloader = html_downloader.HtmlDownLoader()
        # 初始化 html 解析器
        self.parser = html_parser.HtmlParser()
        # 初始化 数据 输出管理器
        self.outputer = html_outputer.HtmlOutputer()

    def craw(self, root_url):
        count = 1
        self.urls.add_new_url(root_url)
        while self.urls.has_new_url():
            try:
                new_url = self.urls.get_new_url()
                html_cont = self.downloader.downloader(new_url)
                new_urls, new_datas = self.parser.parse(new_url, html_cont)
                self.urls.add_new_urls(new_urls)
                print('craw %d : %s' % (count, new_datas))
                self.outputer.collect_data(new_datas)

                if count == 100:
                    break
                count += 1
            except:
                print('craw failed')
        self.outputer.output_html()
        self.outputer.output_mysql()


if __name__ == '__main__':
    root_url = 'http://www.hbooker.com/book/100071888'
    obj_spider = SpiderMain()
    obj_spider.craw(root_url)
```

### 网页下载器 html_downloader

```
# URL下载器
from urllib import request


class HtmlDownLoader(object):
    def downloader(self, new_url):
        if new_url is None:
            return

        response = request.urlopen(new_url)
        if response.getcode() != 200:
            return None
        return response.read()
```
### 网页解析器 html_parser

```
# html 解析器
import re
from bs4 import BeautifulSoup


class HtmlParser(object):
    def _get_new_datas(self, url, soup):
        res_data = {}
        res_data['url'] = url
        # <div class="book-title"><h1>假紫剑圣与B套奶妈</h1>
        title_nodes = soup.find('div', class_='book-title').find('h1')
        res_data['title'] = title_nodes.get_text()
        return res_data

    def _get_new_urls(self, url, soup):
        new_urls = set()
        # <a class="img" href="http://www.hbooker.com/book/100075809" target="_blank">
        links = soup.find_all('a', class_='img')
        for link in links:
            new_d_url = link['href']
            new_urls.add(new_d_url)
        return new_urls

    def parse(self, url, html_cont):
        if url is None or html_cont is None:
            return

        soup = BeautifulSoup(html_cont, 'html.parser', from_encoding='utf-8')
        new_urls = self._get_new_urls(url, soup)
        new_html_cont = self._get_new_datas(url, soup)
        return new_urls, new_html_cont

```

### 输出起 html_outputer
```
# 数据输出
import pymysql

#格式化字符串把字典
def dic2sql(sql, data):
    sql2 = sql % (str(data['url']), str(data['title']))
    return sql2


class HtmlOutputer(object):
    def __init__(self):
        self.datas = []

    def collect_data(self, new_datas):
        if new_datas is None:
            return
        self.datas.append(new_datas)

    def output_html(self):
        fout = open('output.html', 'w')
        fout.write('<html>')
        fout.write('<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />')
        fout.write('<body>')
        fout.write('<table>')

        for data in self.datas:
            fout.write('<tr>')
            fout.write('<td>%s</td>' % data['url'])
            fout.write('<td>%s</td>' % data['title'])
            fout.write('</tr>')

        fout.write('</table>')
        fout.write('</body>')
        fout.write('</html>')
        fout.close()

    def output_mysql(self):

        # 打开数据库连接
        db = pymysql.connect("localhost", "root", "MarkCHYL", "TESTDB")

        # 使用 cursor() 方法创建一个游标对象 cursor
        cursor = db.cursor()

        # 使用 execute() 方法执行 SQL，如果表存在则删除
        cursor.execute("DROP TABLE IF EXISTS HBOOKER")

        # # 使用预处理语句创建表
        sql = """CREATE TABLE HBOOKER (
         TITLE  VARCHAR (200) NOT NULL,
         URL  VARCHAR (200))"""
        print(sql)

        cursor.execute(sql)
        #
        for data in self.datas:
            # SQL 插入语句
            # sql = "INSERT INTO HBOOKER(TITLE, URL) VALUES (\'" + str(data['title']) + "','" + str(data['url']) + "')"
            sql = "INSERT INTO HBOOKER(URL,TITLE) VALUES ('%s','%s')"
            sql1 = dic2sql(sql, data)
            print(sql1)
            try:
                # 执行sql语句
                cursor.execute(sql1)
                # 提交到数据库执行
                db.commit()
            except:
                # 如果发生错误则回滚
                db.rollback()

        # 关闭数据库连接
        db.close()

```
后记：代码可能比较多  我只是为了更好的了解整个爬虫的过程