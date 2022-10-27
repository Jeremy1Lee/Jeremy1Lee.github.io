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

# 添加到顶上的索引

找到 data/archive.yml ，按格式添加其 url ，然后需要写这个 url 对应的界面（md文件），可以用现有的 layouts 等。

目前已经实现了这种方法，并为其新了新的 layouts 模板，命名为archive-net，在新模板中去掉了左侧背景，保留了顶部索引栏。

# 添加到左侧的索引

找到data/navigator.yml，其他内容和添加到顶上一致。

# 更改网站的 Favicon

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











