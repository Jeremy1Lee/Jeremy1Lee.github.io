---
layout: post
title: "常用十种算法"
author: LJC
tags:
- algorithm
- java
date: 2022-10-12 11:20 +0800
toc: true
---
将数据结构使用到实际场景，解决一个问题，重点在于算法。
{: .message }

## 二分查找算法（非递归）

二分查找法只适用于从**有序**的数列中查找（比如数字和字母等），将数列 **排序后** 再进行查找。二分查找法的运行时间为对数时间 O(log2n)，n为数组长度，即查找到目标位置最多只需要 log2n 步。假设从 0~99 的队列（100 个数，即 n = 100），中查找数： 30，则需要查找的步数为 log2 100，即最多需要查找 7 次。

使用**递归**的二分查找算法：

{% highlight js %}
public static int binaSearch(int[] arr, int left, int right, int num){
        if (left <= right) {
            // 当left>right，说明递归整个数组都没找到
            int mid=(left+right)/2; //效率太低，可以使用插值查找
            // mid =left+(right-left)*(num-arr[left])/(arr[right]-arr[left])

            if (arr[mid] == num) {
                return mid;
            } else if (arr[mid] > num) {
                // 注意！这里不写return的后果：即使找到值了也输出-1
                return binaSearch(arr,left,mid-1,num);
            }else if (arr[mid] < num) {
                // 注意！这里不写return的后果：即使找到值了也输出-1
                return binaSearch(arr,mid+1,right,num);
                // 注意！这里要写mid+1，不然永远left==right，且没有该值，跳不出去,stackoverflow
            }
        }
        return -1;
    }
{% endhighlight %}

使用**循环**的二分查找算法：

{% highlight js %}
public static int binSearchLoop(int[] arr, int target){
        int low = 0;
        int high = arr.length-1;
        int mid;
        while (low <= high) {
            mid = (low + high) / 2;
            if (target == arr[mid]){
                return mid;
            }
            if (target < arr[mid]){
                high = mid-1;
            }
            if (target > arr[mid]){
                low = mid+1;
            }

        }
        return -1;
    }
{% endhighlight %}

既然 while 里 low 给了 ≤ high，那么大概率会有 **low = high** 的时候，此时进入循环，mid = low = high ，也就是说，mid 和 high 下标对应的值都被查找过了，也在查找的考虑范围之内。考虑到这个，每次缩小区间的时候只需要将 low 和 high 分别更新成 mid + 1 和 mid - 1 即可，因为 mid 所对应的下标有专属于它的条件判断。

## 分治算法

字面上的解释是 **分而治之**，把一个复杂的问题 **分成两个或更多的相同或相似的子问题，再把子问题分成更小的子问题**.... 直到最后子问题可以简单的直接求解，原问题的解即 **子问题的解的合并**。“分治”是很多高效算法的基础，比如 排序算法（快排、归并排序），傅里叶变换、快速傅里叶变换。

一些可以用分治算法求解的经典问题：

- 二分搜索

- 大整数乘法

- 棋盘覆盖

- 快速排序

- 归并排序

- 线性时间选择

- 最接近点对问题

- 循环赛日程表

- 汉诺塔

最难的地方在于：**如何把复杂的问题拆解成简单的子问题？即：怎么拆？**。其基本步骤为：

- 分解：将原问题分解为若干个规模较小、相互独立、与原问题**形式相同的子问题**；

- 解决：若子问题规模较小而容易被解决则直接解决，否则递归的解各个子问题；

- 合并：将各个子问题的解合并为原问题的解；

分治算法设计模式如下：

当 P 的规模不超过 n0 时，直接用分治法中的基本子算法 ADHOC(P) 求解。否则，分解，递归，合并。

{% highlight js %}

if |P| <= n0 // 规模很小
    then return(ADHOC(P))
for i ← 1 to k // 分解
do yi ← Divide-and-conquer(Pi) // 递归解决
T ← MERGE(y1, y2, ..., yk) //合并子问题
return(T)
    
{% endhighlight %}

### 汉诺塔（Tower of Hanoi）

[汉诺塔：](https://baike.baidu.com/item/%E6%B1%89%E8%AF%BA%E5%A1%94/3468295?fr=aladdin#3_2) 三根柱子，在一根柱子上从下往上按照大小顺序摞着 64 片盘。把圆盘从下面开始按大小顺序重新摆放在另一根柱子上。规定，在小圆盘上不能放大圆盘，一次只能移动一个圆盘。

假设初始有 n 个盘，无非是 n-1 个复合盘和 1 个大盘的两盘问题，而 n-1 个盘又是 n-2 个复合盘和 1 个大盘的两盘问题 ... 直到只有两个盘，这非常简单。试图推敲 n 个盘（n大于5）非常困难，人脑也会栈溢出，但是递归会一直不厌其烦地将 n 个盘解决到两个盘的情况，进而全盘皆活。

思路分析（柱子 A，B，C ）：

1. 如果有一个盘： A => C；

2. 如果有 n (n>=2) 个盘，我们总是可以看成两个盘，一个是最下面的最大盘，另一个是上面的所有 n-1 个复合盘：

1）先把上面的复合盘从 A => B；

2）把最下面的最大盘从 A => C；

3）把复合盘从 B => C；

代码非常简单，如下：

{% highlight js %}
// a和c分别表示起，始，b表示中转；用一条打印语句表示挪盘操作
public static void hanoiTower(int num, char a, char b, char c){
        if (num == 1) {
            System.out.printf("第 1 个盘从 %s → %s \n", a, c);
        }else {
            // 如果有 n (n>=2) 个盘，我们总是可以看成两个盘，一个是最下面的最大盘，另一个是上面的复合盘：
            //1）先把上面的复合盘从 A => B；现在a和b是起始，c是中转，数量是n-1个
            hanoiTower(num-1,a,c,b);
            //2）把最下面的最大盘(第num个) 从 A => C；
            System.out.printf("第 %d 个盘从 %s → %s \n", num, a, c);
            //3）把复合盘从 B => C；
            hanoiTower(num-1,b,a,c);
        }
    }
{% endhighlight %}

只考虑1和2的情况：首先如果只有一个盘，直接把它从A挪到C就行了。如果有两个盘：先把上盘从A挪到B中转，然后下盘从A挪到C，然后上盘从B挪到C。从1到2是这样，从n-1到n也如此。把1和2替换成num和num-1，就得到了递归程序。

试图提炼一下这里的思想：

- 一定要想到边界：即开始、结束，以及中间的操作。

- 必须明确我这个方法输入什么，会得到什么输出/什么结果，并且认为这个方法已经写好了，可以直接拿来用。每次给方法变化的输入，得到变化的输出。

- 每次执行操作后变化的是什么？这个变化的量何时停下来？（这里是当 num==1 不再执行递归）

### 关于递归，多说两句

递归是 *A Leap of Faith* 。写递归，我只需要写两个case：一个case规定边界条件（跳出的base case）、一个case规定递归操作 (recursive case) 。在 recursive 中，我们信任递归已经给出了正确的 n-1 返回值，利用它去构建 n 的返回值。不要试图推演递归调用，直接信任它，并在其基础上修改从n到n+1的操作，*leave the hard work to the computer*。 一定要推演的话可以推演一个递归深度浅的例子。我从大运村走到图书馆，我只需要考虑跳出递归的最后一步：迈完这一步，我就进入到图书馆了，比如距离等；以及递归操作：迈步。调用这个程序时，你只管迈出第一步，剩下的交给递归。


&emsp;&emsp;1

&emsp;&emsp;5

{% highlight js %}

{% endhighlight %}
