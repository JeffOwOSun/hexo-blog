title: 极影监视器-设想
id: 18
categories:
  - 'Coding & Geek Stuff'
date: 2014-10-14 19:39:46
tags:
---

每次都上极影查看有没有新资源真是十分蛋疼啊。看到极影有rss源，准备自己写一个脚本抓取新出的资源。

语言：看看什么库好用再决定吧。感觉python比较靠谱。

要实现的功能：

1.  监视rss源的变化
2.  自定义规则，为一系列同类资源进行排序
3.  调用下载器进行下载
4.  邮件通知

    1.  有新资源的时候
    2.  下载完成的时候
2014-10-26update: 发现了一个很有用的解析rss的下载工具叫flexget: [http://blog.binux.me/2011/12/from_xunlei_lixian_for_flexget/](http://blog.binux.me/2011/12/from_xunlei_lixian_for_flexget/)

大概是根据input指定的rss抓取网页内容搜寻到种子然后丢给output下载。
