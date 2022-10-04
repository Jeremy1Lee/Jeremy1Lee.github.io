---
layout: post
title: "二叉排序/搜索树(BST)"
author: LJC
tags:
- Java
date: 2022-10-02 20:00 +0800
toc: true
---
## 背景
> 需求：一个数列 {7, 3, 10, 12, 5, 1, 9} ，要求能够高效的完成对数据的**查询和添加**。

&emsp;&emsp;1）使用数组实现：
- 数组未排序的话，添加直接在数组尾添加，速度快，但缺点是查找速度慢
- 数组排序了，可以用二分查找，速度快，但缺点是添加时找到位置后数据整体移动，速度慢。

&emsp;&emsp;2）使用链表实现：不管链表是否有序，查找速度都慢，添加速度快（不需要整体移动）。

&emsp;&emsp;3）使用二叉排序树：则增/删/改/查都很快。

## 二叉排序/搜索树(Binary Sort/Search Tree)

&emsp;&emsp;任何一个非叶子节点，要求左子节点的值比当前节点的值小，右子节点的值必当前节点的值大。注意，如果有相同的值，可以将该节点放在左子节点或右子节点，但最理想的还是没有重复值。

&emsp;&emsp;如图，检索效率类似于二分查找，在添加 2 节点时，找到添加位置后，添加也类似链表的挂载，删除速度也很快。
![bst01.png](/images/bst01.png "BST"){: .align-center}

### BST相关代码

### 建立 BST ，重载是个好东西

一个数组创建成对应的二叉排序树，显然二叉排序树的顺序与中序遍历对应，使用中序遍历二叉排序树。

1. 判断是左还是右？
2. 左右现在有无节点？

&emsp;&emsp;2.1 无节点，直接挂上

&emsp;&emsp;2.2 有节点，继续以这个节点作为根寻找其左右（递归）

{% highlight js %}
    public void add(Node newNode){
        if (newNode == null){
            return;
        }
        // 判断传入的节点的值和当前子树的根节点的值的关系
        if (newNode.value < this.value) {
            if (this.left == null) {
                this.left = newNode;
            }
            else {
                this.left.add(newNode);
            }
        }
        if (newNode.value > this.value) {
            if (this.right == null) {
                this.right = newNode;
            }
            else {
                this.right.add(newNode);
            }
        }
    }
{% endhighlight %}

### BST 删除节点

&emsp;&emsp;BST删除节点有三种情况：

![bst02.png](/images/bst02.png "BST"){: .align-center}

- （一）删除**叶子节点**（蓝）

删除思路：

1）先去找到需要删除的节点 targetNode

2）找到 targetNode 的父节点 parentNode 

3）确定 targetNode 是 parentNode 的左子节点还是右子节点以确定删除

4）根据前面的情况对应删除：

&emsp;&emsp;左子节点： parentNode.left  = null;

&emsp;&emsp;右子节点： parentNode.right = null;

- （二）删除**只有一颗子树的节点**（黄）

删除思路：

1）先去找到需要删除的节点 targetNode

2）找到 targetNode 的父节点 parentNode 

3）确定：【1】 targetNode 的子节点是左子还是右子，以及【2】 targetNode 是 parentNode 的左子还是右子。出现四种情况：判断，并对应挂载即可

![BSTdel01.png](/images/BSTdel01.png "BST-delete-situation2"){: .align-center}

&emsp;&emsp; 3.1：targetNode 是 parentNode 的**左**孩子，且 targetNode 的孩子是**左**孩子：parentNode.left = targetNode.left

&emsp;&emsp; 3.2：targetNode 是 parentNode 的**左**孩子，且 targetNode 的孩子是**右**孩子：parentNode.left = targetNode.right

&emsp;&emsp; 3.3：targetNode 是 parentNode 的**右**孩子，且 targetNode 的孩子是**左**孩子：parentNode.right = targetNode.left

&emsp;&emsp; 3.4：targetNode 是 parentNode 的**右**孩子，且 targetNode 的孩子是**右**孩子：parentNode.right = targetNode.right

- （三）删除**有两颗子树的节点**（红）

删除思路：

1）先去找到需要删除的节点 targetNode

2）找到 targetNode 的父节点 parentNode **（但有可能没有父节点，即根节点就没有父节点）**

3）找到 targetNode 的最小子树（左子树）

4）用一个临时变量 temp 将这个最小值节点保存

5）删除该最小值节点

6）把 temp 赋给 target

![bstdel02.png](/images/bstdel02.png "BST-delete-all-situation"){: .align-center}

&emsp;&emsp;删除节点的代码实现：
{% highlight js %}

{% endhighlight %}
