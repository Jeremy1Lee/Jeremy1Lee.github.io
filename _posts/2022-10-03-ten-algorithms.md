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

既然 while 里 low 给了 ≤ high，那么大概率会有 **low = high** 的时候，此时进入循环，mid = low = high ，也就是说，mid 和 high 下标对应的值都被查找过了，也在查找的考虑范围之内。考虑到这个，每次缩小区间的时候只需要将 low 和 high 分别更新成 mid + 1 和 mid - 1 即可，因为 mid 有专属于自己的条件判断。






















## 背景
> 1

&emsp;&emsp;1

&emsp;&emsp;2
![lay.jpg](/images/lay.jpg "LAY"){: .align-center}

{% highlight js %}

{% endhighlight %}
