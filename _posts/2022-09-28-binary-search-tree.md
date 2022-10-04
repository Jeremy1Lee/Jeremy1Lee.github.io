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

## BST相关代码

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

### 删除 BST 非根节点，递归时用不用 return ，区别很大！

&emsp;&emsp;删除 BST 的非根节点有三种情况：

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

1）先去找到需要删除的节点 targetNode

2）找到 targetNode 的父节点 parentNode 

3）确定：【1】 targetNode 的子节点是左子还是右子，以及【2】 targetNode 是 parentNode 的左子还是右子。出现四种情况：判断，并对应挂载即可

![BSTdel01.png](/images/BSTdel01.png "BST-delete-situation2"){: .align-center}

3.1：target 是 parent 的**左**孩子，且 target 有**左**孩子：parentNode.left = targetNode.left

3.2：target 是 parent 的**左**孩子，且 target 有**右**孩子：parentNode.left = targetNode.right

3.3：target 是 parent 的**右**孩子，且 target 有**左**孩子：parentNode.right = targetNode.left

3.4：target 是 parent 的**右**孩子，且 target 有**右**孩子：parentNode.right = targetNode.right

- （三）删除**有两颗子树的节点**（红）

1）先去找到需要删除的节点 targetNode

2）找到 targetNode 的父节点 parentNode **（但有可能没有父节点，即根节点就没有父节点）**

3）找到 targetNode 的最小子树（左子树）

4）用一个临时变量 temp 将这个最小值节点保存

5）删除该最小值节点

6）把 temp 赋给 target

![bstdel02.png](/images/bstdel02.png "BST-delete-all-situation"){: .align-center}

### 考虑上删除根节点的情况后，整个删除节点的流程

![chart06.png](/images/chart06.png "flowchart-BSTdel"){: .align-center}

&emsp;&emsp;有了上述非根节点删除的基础思想后，我们考虑加入删除根节点的情况，最后如上图所示，首先从根节点开始找待删节点，并判断待删节点的类型：如果删除的非根节点，则还有三种情况：上面的删除思路里详细介绍了这三种删除情况。但如果待删节点是根节点，则同样有三种情况，对应树的形状如左侧黄/红色所示，蓝色为删除根节点条件下寻找继承的新根的方法，代码如下：

{% highlight js %}
public Node searchSubRoot() {
        // 2.1 2.2，2.3 情景下 寻找继承根节点 的节点 的方法，一一对应的图示见蓝色
        if (this.right != null) {
            if (this.left == null) {
                // 2.2 右继承
                rootFlag = true;// 这里的 rootFlag 是旧 root 的成员
                return this.right;
            }else {
                // 2.3 右子树最左继承
                Node startNode = this.right;
                while (startNode.left != null) {
                    startNode = startNode.left;
                }
                return startNode;
            }
        } else if (this.left != null) {
            // 2.2 左继承
            rootFlag = true;
            return this.left;
            }else{
            // 2.1 无继承
            return null;
        }
    }
{% endhighlight %}

&emsp;&emsp;如果是图中 2.1 ，直接删成空树即可，如果是图中 2.3 ，思路是先找到新根并持有，删除新根，新根的值覆盖旧根。值得一提的是 2.2 删除代码，如下所示：注意 rootFlag 是在 root 里的；只有在2.2场景下才会涉及到rootFlag，因此判断并新旧root删除，具体删除图示如蓝色。

{% highlight js %}
// 2.2 只有根的一边有子树，图示见蓝色
        if (root.rootFlag){
            System.out.println("正在删除根节点，此时为单边树...");
            // 持有旧root
            Node tempRoot = root;
            // 设置新root
            setRoot(newRoot);
            // 删掉旧root
            tempRoot.right = null;
            tempRoot.left = null;
            // 无需重置 rootFlag ，因为 rootFlag 是旧 root 的成员，而且已经被删掉了
            return;
        }
{% endhighlight %}

## 总结

这部分代码的体会：

1） 注意下面的<mark> return </mark>：

{% highlight js %}
public Node searchParentNode(int value) {
        // 先写 this.left != null 防止nullpointer
        if ((this.left != null && this.left.value == value) || (this.right != null && this.right.value == value)) {
            return this;
        }
        if (value < this.value && this.left != null) {
            // 这里一定要return！！！！不然永远都会执行到 下面的return null，怎么都是null！
            return this.left.searchParentNode(value);
        }
        if (value >= this.value && this.right != null) {
            // BST 要尽量避免相同值，根据add()，≥都放在右了，所以这里也去右边找；
            return this.right.searchParentNode(value);
        }
        return null;
    }
{% endhighlight %}

2）类中**成员 Boolean 变量**的设置，方便条件判断，见上面的<mark> rootFlag </mark>。