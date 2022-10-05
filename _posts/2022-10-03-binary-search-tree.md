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

## 相关代码

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

&emsp;&emsp;有了上述非根节点删除的基础思想后，我们考虑加入删除根节点的情况，最后如上图所示，首先从根节点开始找待删节点，并判断待删节点的类型：若删除非根节点，上面详细介绍了三种删除非根节点的情况。如果删除根节点，则同样有三种情况，对应树的形状如图中左侧所示，特别的，蓝色为删除根节点条件下寻找继承的新根的方法，代码如下：

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

&emsp;&emsp;如果是图中 2.1 ，直接删成空树即可，如果是图中 2.3 ，则先找到新根节点并持有它，删除新根节点，刚刚持有的新根节点的值覆盖旧根即可。值得一提的是 2.2 场景的删除代码，如下所示：注意 rootFlag 是在 root 里的；只有在 2.2 场景下才会涉及到rootFlag，由此作出对 2.2 场景下**单边树**的删除操作，操作图示如上图右侧蓝色所示。

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

高效的数据增，删，改，查

二叉排序树**增，删，改，查**代码的体会：

1） 注意下面的 **return语句**：如果不**return**，而是直接 this.left.searchParentNode(value) ，会怎么样？

{% highlight js %}
public Node searchParentNode(int value) {
        // 先写 this.left != null 防止nullpointerE
        if ((this.left != null && this.left.value == value) || (this.right != null && this.right.value == value)) {
            return this;
        }
        if (value < this.value && this.left != null) {
            // 这里一定要return！！！！不然永远都只会执行到 下面的return null，怎么都是返回null！导致异常！
            return this.left.searchParentNode(value);
        }
        if (value >= this.value && this.right != null) {
            return this.right.searchParentNode(value);
            // 如果不写 return 会怎么样？
        }
        return null;
    }
{% endhighlight %}

2）活用类中**成员 Boolean 变量**，方便条件判断，见上面的 **rootFlag** 。

3）一些方法中用到了多个**if**，一定程度上影响了程序的可读性和可维护性，解决办法之一是对三种情况用一个变量表征，并用不同方法处理。


## 平衡二叉树（ AVL树 ）

### 基本介绍

背景：数列 [ 1, 2, 3, 4, 5, 6 ]对应的二叉排序树形状，如图所示：

![avlTree01.png](/images/avlTree01.png "AVL-Tree"){: .align-center}

&emsp;&emsp;显然，树形状和单链表非常像，虽然增加节点不影响，但**查询**速度没有发挥出二叉排序树应有的优势，甚至由于每次比较左子树，查询速度比单链表还慢。解决方法：**平衡二叉树（AVL）**，作为对 BST 的优化。

&emsp;&emsp;平衡二叉树又叫平衡二叉搜素树(Self-balancing binary search tree)，又称 AVL 树，可以**保证查询效率较高**。其特点为：1）是一颗空树/或者**左右两个子树层数差的绝对值不超过1**，2）左右子树都是平衡二叉树。平衡二叉树的实现方法有红黑树，AVL 算法，替罪羊树， Treap ，伸展树等。**注意前提：首先必须是二叉排序树 BST ，然后再看左右子树判断是否为 AVL 树**。搜索、添加、删除的时间复杂度是O(logn)。

### 单旋转：左旋与右旋

&emsp;&emsp; AVL 树中的每个节点都有一个平衡因子（Balance Factor），即左右子树高度之差。在添加节点后，树中一些节点会失衡，要对这棵 AVL 树进行调整，但进行的调整也不能太大动干戈，尝试用尽量的调整使树回到平衡状态，否则就失去了降低复杂度的初衷。在 AVL 树中，采用**左旋**或者**右旋**的操作对失衡的节点进行调整。思路是：由于失衡的一系列节点只能可能是插入节点的祖父节点或祖祖…父节点，我们从插入节点出发，一路向上找到**第一个失衡**的祖先节点，然后对以该节点为根节点的二叉树进行**一次左旋或者一次右旋或者双旋转**调整，将其调整为平衡状态，并将高度恢复至与原来相同，那么该节点已恢复平衡，且更上层的祖先节点由于其高度与原来一致，于是也恢复到了平衡状态。

![avlLeftB01.png](/images/avlLeftB01.png "AVL左旋转"){: .align-center}

例如，一个数列 4,3,6,5,7,8 ，以其对应的二叉搜索树，创建出它对应的平衡二叉树，左旋的过程如上图所示：

&emsp;&emsp;更一般性的左右旋过程对比，以及旋转调整前后的树形状，节点位置对比，以及旋转操作的原理如下图所示，由图可以得出结论：

- **左旋可以使开始节点的左子树高度 +1 ，右子树高度 -1** ；

- **右旋可以使开始节点的左子树高度 -1 ，右子树高度 +1** ；

![avlLeftB02.png](/images/avlLeftB02.png "详细的左右旋"){: .align-center}

&emsp;&emsp;究竟是使用左旋，还是右旋，取决于**新增节点在第一个失衡节点的 __子树 的 __子树**，也就是说有四种情况：

1. 新增节点在第一个失衡节点的**左子树**的**左子树**，简称 *LL*：第一个失衡节点右旋；

2. 新增节点在第一个失衡节点的**右子树**的**右子树**，简称 *RR*：第一个失衡节点左旋；

3. 新增节点在第一个失衡节点的**右子树**的**左子树**，简称 *RL*：第一个失衡节点右子节点左旋 + 第一个失衡节点右旋；

4. 新增节点在第一个失衡节点的**左子树**的**右子树**，简称 *LR*：第一个失衡节点左子节点右旋 + 第一个失衡节点左旋；

![fourAvlTree.png](/images/fourAvlTree.png "四种情形"){: .align-center}

### 双旋转：左右旋与右左旋

&emsp;&emsp;上面介绍了何时单旋转，何时双旋转，下面是使用双旋转的场景以及双旋转的图示，可以看到，双旋转经过一次旋转操作可以转换为单旋转的场景。需要注意的是：双旋操作第一次旋转是对第一个失衡节点的**子节点**而言的，而第二次旋转才是对第一个失衡节点本身操作。

![doubleAvlTree.png](/images/doubleAvlTree.png "双旋转"){: .align-center}

