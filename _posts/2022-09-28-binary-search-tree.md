---
layout: post
title: "二叉排序/搜索树(BST)"
author: LJC
tags:
- Java
date: 2022-09-28 10:00 +0800
toc: true
---
## 背景
需求：一个数列{7, 3, 10, 12, 5, 1, 9}，要求能够高效的完成对数据的查询和添加。

&emsp;&emsp;使用数组实现：
- 数组未排序的话，添加直接在数组尾添加，速度快，但缺点是查找速度慢
- 数组排序了，可以用二分查找，速度快，但缺点是添加时找到位置后数据整体移动，速度慢。

&emsp;&emsp;使用链表实现：不管链表是否有序，查找速度都慢，添加速度快（不需要整体移动）。

&emsp;&emsp;使用二叉排序树：则增/删/改/查都很快。

## 二叉排序/搜索树(Binary Sort/Search Tree)

&emsp;&emsp;任何一个非叶子节点，要求左子节点的值比当前节点的值小，右子节点的值必当前节点的值大。注意，如果有相同的值，可以将该节点放在左子节点或右子节点。

&emsp;&emsp;如图，检索效率类似于二分，在添加 2 节点时，找到添加位置后，添加也类似链表的挂载，删除速度类似。
![bst01.png](/images/bst01.png "BST"){: .align-center}

### 代码建立 BST，重载是个好东西


{% highlight js %}

{% endhighlight %}


&emsp;&emsp;
{% highlight js %}

{% endhighlight %}
