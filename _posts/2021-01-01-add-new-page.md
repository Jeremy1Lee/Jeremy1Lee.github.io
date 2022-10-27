---
layout: post
title: "网站模板"
author: LJC
tags: toturial
date: 2021-01-01 10:00 +0800
toc: true
---
向网站中添加 items
{: .message }

## 继承关系

![web01.png](/images/web01.png "web"){: .align-center}


以下是目录，及左右布局的继承关系，其他布局还在研究中：
![web02.png](/images/web02.png "web"){: .align-center}

## 添加到顶上的索引

找到 data/archive.yml ，按格式添加其 名称和 URL ，注意需要提前写好这个 url 对应的界面（md文件），md 可以套用现有的 layouts 。

目前已经实现了这种方法，并为其写了新的 layouts 模板archive-net，在新模板中去掉了左侧背景，保留了顶部索引栏。

## 添加到左侧的索引

找到data/navigator.yml，其他内容和添加到顶上一致。

## 更改网站的 Favicon

1. [Favicon生成网站](https://realfavicongenerator.net/)
2. 下载压缩包，解压到网站**根目录**下，并替换以前的favicon文件；

3. 以下代码加到 Jeremy1Lee.github.io\_includes\custom-head.html 文件   

{% highlight js %}
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
<link rel="manifest" href="/site.webmanifest">
<meta name="msapplication-TileColor" content="#da532c">
<meta name="theme-color" content="#ffffff">
{% endhighlight %}











