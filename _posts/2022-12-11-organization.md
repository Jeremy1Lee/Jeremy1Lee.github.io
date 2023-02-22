---
layout: post
title: "组成原理/硬件知识"
author: LJC
tags:
- organization
date: 2022-12-11 09:10 +0800
toc: true
---

# 计算机基础系列

- 计算机的历史：机械传动（齿轮、继电器等）→真空管（机电）→晶体管（微观）

怎么使用**晶体管**完成计算，尤其是在没有齿轮等机械的情况下？
- 底层：T和F 用 1和0 表示
- 引入布尔代数：

1. 门电路：可以用true和false进行逻辑运算：基本的与或非、异或XOR、 ......

2. 锁存器：或门output和input连起来，可以永久保存1；与门连起来永远保存1；两者结合得到锁存器，锁定一个bit，从此可读可写。存住一位0或1，8个锁存器串行，允许表示8位2进制数（也得益于二进制）

3. 内存和内存地址：矩阵形式放置锁存器，如何找到某个锁存器？，根据其结构，假设一次能操作8位，结合矩阵形式的锁存器阵 控制操作哪8个位，需要地址线、数据线、读写控制线等，内存可以随时访问任何位置——“随机”--- RAM


计算机如何存储和表示数字？
- 二进制：TRUE和FALSE表示1和0，位数越多可表示的数字越大；
- 正负：反码和**补码**；浮点数：分成正负（1）、指数（8）、小数（23）存；
- 文字：给字母符号编号用0和1表示更多符号：ASCII/ UTF-8；

计算机怎么操作二进制/计算机如何作加减法？首先，**ALU**/算术逻辑单元 是计算机中负责运算的组件；
- 加法一共四种：0+0=0，1+0=1，0+1=1都可以用XOR门实现，但1+1=0要进位，于是在XOR门加入AND门表示进位；
    - 半加器和全加器：半加器两个输入（两个输出），全加器三个输入（得到进位）
- 减法需要计算负数的补码（反码+1），然后正数和负数的补码相加，丢弃溢出位，就是减法运算

# 图解系统

CPU找内存中的数据时，先从Cache中找，是通过内存地址找到 CPU Cache 中的数据，而内存地址与Cache数据块有映射关系：

CPU要访问某个内存地址，首先通过该数据的 内存地址 计算对应的 Cache Line地址（即：如果cache里有该内存数据，它应该在Cache的这个位置存着）

![CPUwithCache.png](/images/os/CPUwithCache.png "CPU")

CPU Cache 的数据写入：**写直达和写回**；
- 写直达：每次写操作都把数据写回到内存；
- 写回：新的数据仅仅被写入 Cache Block 里，只有当修改过的 Cache 数据块**将要被替换**成其他数据时，才需要写到内存中。如果我们大量的操作都能够命中缓存，那么大部分时间里 CPU 都不需要读写内存，自然性能相比写直达会高很多。

**多核**CPU的Cache如何保持缓存一致性（不然多个核心同时操作一个变量，这个变量就混乱了）：**MESI协议**：基于总线嗅探机制，实现了事务串行化，也用状态机机制降低了总线带宽压力；

![Memory_and_Cache.png](/images/os/Memory_and_Cache.png "内存和Cache")

CPU伪共享（False Sharing）：多个线程同时读写同一个 Cache Line 的不同变量时，CPU Cache 失效的现象（这是因为受MESI协议影响，一个核心写数据，其他核心中的Cache会失效，如此需要多次写回和读取内存，Cache的意义也没了，降低了速度）；
- 那么如何解决？让两个连续变量占不同Cache Line即可：本质上是空间换时间


-----------






