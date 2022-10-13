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

## 一、二分查找算法（非递归）

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

## 二、分治算法

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

### 汉诺塔 (Tower of Hanoi)

&emsp;&emsp;[汉诺塔](https://baike.baidu.com/item/%E6%B1%89%E8%AF%BA%E5%A1%94/3468295?fr=aladdin#3_2) ：三根柱子，在一根柱子上从下往上按照大小顺序摞着 64 片盘。把圆盘从下面开始按大小顺序重新摆放在另一根柱子上。规定，在小圆盘上不能放大圆盘，一次只能移动一个圆盘。

&emsp;&emsp;假设初始有 n 个盘，无非是 n-1 个复合盘和 1 个大盘的两盘问题，而 n-1 个盘又是 n-2 个复合盘和 1 个大盘的两盘问题 ... 直到只有两个盘，这非常简单。试图推敲 n 个盘（n大于5）非常困难，人脑也会栈溢出，但是递归会一直不厌其烦地将 n 个盘解决到两个盘的情况，进而全盘皆活。

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

汉诺塔只是一个场景之一，并且明确了告诉用递归、怎么递归（拆成 n-1 和1），难的地方也正是如何想到把它拆分成 n-1 和 1 。**学算法要学思想，不然会学成八股文：见过的能想起来，没见过的怎么都想不起来**。

### 关于递归，多说两句

&emsp;&emsp;递归是 *A Leap of Faith* 。写递归，我只需要写两个case：一个case规定边界条件（跳出的base case）、一个case规定递归操作 (recursive case) 。在 recursive 中，我们信任递归已经给出了正确的 n-1 返回值，利用它去构建 n 的返回值。不要试图推演递归调用，直接信任它，并在其基础上修改从n到n+1的操作，*leave the hard work to computer*。 一定要推演的话可以推演一个递归深度浅的例子。我要走1000米，我只需要考虑跳出递归的最后一步：迈完这一步里程就够1000米了；以及递归操作：迈步。调用这个程序时，你只管迈出第一步，剩下的交给递归。
其实可以说，设计什么程序都是在由小至大：比如[接雨水](https://leetcode.cn/problems/trapping-rain-water/)，我考虑一个列的情况：这个列要么左右都不空，显然是min(左，右)-本列高，要么左右有一空，是min(左，右面最高列) - 本列高，要么左右都空，是min(左面最高列，右面最高列) - 本列高。一个列的算法搞定了，推广至所有列，除了第一列和最后一列不可能接到以外。递归也好，其他算法也罢，算法要学思想：一种解决问题的条件反射能力，也就是让解法变得“高级”，递归就很高级。什么问题都是暴力法，那是真的白学了；而各种数据结构则要做到熟悉，不然用哪个都不知道。

## 三、动态规划 (Dynamic Programming)

应用场景 —— 背包问题：给定容量的背包、若干具有一定价值和重量的物品，如何选择物品放入背包使物品的价值最大。（01 背包：放入物品不能重复；无限背包：放入物品可重复，无限背包可以转换成01背包）

> 背包能装 4kg 东西。吉他 1kg，价值 1500；  音响 4kg，价值 3000； 电脑 3kg，价值 2000；要求：重量不超的情况下，装入背包的总价值最大，并且不能重复。

### 动态规划解决背包问题

&emsp;&emsp;动态规划算法：将大问题划分为小问题进行解决，从而一步步获取最优解的处理算法。显然基本思想和 DivideAndConquer 类似，不过不同的是适合动态规划求解的问题，**经分解得到的子问题不是独立的**。分治法中把复合盘放到C对最大盘没有影响，二者是独立的；但背包就这么大，放入一个物品，其他的必会受到影响。也就是说，**动态规划中下一个子阶段的求解是建立在上一个子阶段的解的基础上，进行下一步求解**。下一个解会参考当前的解。动态规划可以通过**填表**的方式来逐步推进，得到最优解。

<mark>解算法题，一定要有**思想**！然后转化成公式或规律，最后转化成你的程序。</mark>

背包问题利用动态规划来解决。每次遍历到的**第 i 个物品**，根据**物品重量 w[i]** 和**物品价值 v[i]** 来确定是否需要将该物品放入背包中。假设当前已经放了 i 个物品在背包中，那么当前背包的容量为 j，能够放进去的容量用 v[i][j] 表示。即 v[i][j] 表示了能装入容量为 j 的背包中的最大价值，由于 w 和 v 都是变的，就是它在动态变化。

下面用填表法逐步推导规律和公式：表格是一行一行填的，横向表示不同容量的包，纵向表示当前提供往里装的物品，表格表示背包总价值。第一行表示没有提供任何物品时，不管如何，价值都是0，第二行表示提供G在不同背包容量下的最大价值，以此类推，这张表是所有情况的最大值。设计算法也要考虑这张表的来源：下一行是基于上一行填的，一行一行来。

![dynamic01.png](/images/dynamic01.png "动态规划-填表法"){: .align-center}

这样，每一列的最后一行是该背包容量下价值最大的解。表格的核心是**三条规则**：

{% highlight js %}
        // 行 i 表示每次提供新物品；列 j 表示每次更新的背包大小
    v[i][0] = v[0][j]=0 ;
        // 表示第一行和第一列都是0，即包容量为0，价值为0；不提供物品，价值为0。（第一列与第一行）

    当 w[i] > j 时：v[i][j] = v[i - 1][j] ;
        // 光新提供的物品一个就大于整个包大小了，提不提供它和现在大小的包没关系，直接把同一列上一行赋给它即可（红色箭头）

    当 j ≥ w[i] 时：v[i][j] = max{v[i - 1][j] , v[i - 1][j - w[i]] + v[i]} ;
        // 当新提供的物品容量小于等于现在包的大小（其他东西还能装），此时装包有两种策略：
        // 一是无视新物品，还和上一行一样装；二是先装这个新物品，按包剩下的大小再去上一行找怎么装出最大价值，两者取其大。
        // 策略二体现了对上一行的依赖；
{% endhighlight %}

动态规划，相当于用空间存放了递归的结果，换取时间。代码就是建立表格的过程，首先第一列和第一行肯定全是0，其次，要么复制上一行，要么分类，这取决于新来物品的重量。增加一个打印放了什么物品的功能，建立表格的过程无非就是两种操作：一是复制上一行，另外就是新增一个+在上一行找对应的格子，根据不同建表操作设置打印操作：复制上一行就递归上一行，新增节点，就先打印新增，再递归上一行对应的格子。最后什么都没有的，也就是第一行和第一列，打印背包是空的。

![flowChart04.png](/images/flowChart04.png "flowChart"){: .align-center}

{% highlight js %}
// 递归打印背包信息
public static void calP(int[][] path, int i, int j){
        String[] s = {"G","S","L"};
        int[] w = {1,3,4};// 物品重量
        if (path[i][j] == 1) {
            calP(path,i-1,j);
        } else if (path[i][j] == 2) {
            System.out.print("包里放了"+s[i-1]+", ");
            calP(path,i-1,j-w[i-1]);
        }else {
            System.out.print("包里没东西了");
        }
    }
{% endhighlight %}

## KMP 算法

字符串匹配问题：

> str1 = "硅硅谷 尚硅谷你尚硅 尚硅谷你尚硅谷你尚硅你好"； str2 = "尚硅谷你尚硅你"

求：str2 在 str1 中是否存在，如果存在，返回第一次出现的位置，如果没有则返回 -1。

**暴力匹配法**：假设 str1 匹配到 i 位置，子串 str2 匹配到 j 位置，则：如果当前字符匹配成功（str1[i] == str2[j]），然后 i 和 j 同时往后移动，如果某次移动后失配，则 i 回溯到 移动前的后一位。实现代码如下：

{% highlight js %}
public static int violenceMatch(String motherStr, String childStr){
        char[] mother = motherStr.toCharArray();
        char[] child = childStr.toCharArray();
        int motherLength = mother.length;
        int childLength = child.length;
        int motherCnt = 0;
        int childCnt = 0;

        while (motherCnt < motherLength) {
            while (mother[motherCnt] == child[childCnt]){
                motherCnt ++;
                childCnt ++;
                if (childCnt == childLength){
                    return motherCnt - childLength;
                }
            }
            motherCnt = motherCnt - childCnt + 1;
            childCnt = 0;
        }
        return -1;
    }
{% endhighlight %}

【为什么要回溯？】回溯相当于每次都只前进一个 char ，如果不回溯，而是继续从失败的位置开始匹配，一些出现循环的字符串就会被跳过，如图所示。暴力算法为了避免这种情况，一次只前进一个字符，每个字符为开头的情况都被考虑到了。

![kmp01.png](/images/kmp01.png "KMP"){: .align-center}

**KMP 算法**：KMP 是一个解决**模式串在文本串中是否出现过**，如果出现过，则返回最早出现的位置的经典算法。常用与在一个文本字符串 S 内查找一个模式串 P 的出现位置。KMP 方法利用 之前判断过的信息，通过一个 next 数组，保存模式串中前后最长公共子序列的长度，每次回溯时，通过 next 数组找到前面匹配过的位置，省去了大量的计算时间。




图示如下：

![kmp02.png](/images/kmp02.png "KMP"){: .align-center}


![kmp03.png](/images/kmp03.png "KMP"){: .align-center}







{% highlight js %}

{% endhighlight %}


