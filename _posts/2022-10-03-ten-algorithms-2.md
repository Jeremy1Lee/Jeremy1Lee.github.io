---
layout: post
title: "常用十种算法（二）"
author: LJC
tags:
- algorithm
- java
date: 2022-10-15 21:10 +0800
toc: true
---
将数据结构使用到实际场景，解决一个问题，重点在于算法。（2）
{: .message }

## 五、普里姆 (Prim) 算法

应用场景：修路问题

有 7 个村庄 (A, B, C, D, E, F, G) ，现在需要修路把 7 个村庄连通，各个 **村庄的距离用边线表示(权)**，比如 A – B 距离 5 公里。如何修路保证各个村庄都能连通，并且**修建公路 总里程最短**?

思路：只满足连通的话，将 10 个点全连上就行了，但总的里程数肯定不是最小。满足连通，又保证总里程最短的思路：尽可能少修路，如果不得不修，也要修短的路，保证总里程数最少。

![mst01.png](/images/mst01.png "MST"){: .align-center}

### 最小生成树：由图到树

修路问题的本质/建模，就是最小生成树问题。最小生成树 (Minimum Cost Spanning Tree)，简称 **MST**。给定一个**带权的无向连通图**，如何选取一棵生成树，使树上所有边上权的总和为最小，这叫最小生成树 ，它有如下特点：

-  N 个顶点，一定有 N - 1 条边（如右图考虑了一种最最简陋的连接方式）

- 包含全部顶点（所有节点都可达）

- 树的 N-1 条边都在图中

求最小生成树的算法主要是 **普里姆算法** 和 **克鲁斯卡尔算法**。既然是由图到的树，而且图（多对多）更符合现实，这表明和其他树结构比，其应用范围更广。

### 普利姆 (Prim) 算法

普利姆（Prim）算法求最小生成树，也就是：在包含 n 个顶点的连通图中，找出只有 (n-1) 条边包含所有 n 个顶点的连通子图，也就是所谓的**极小连通子图**。

1. 设：
- G=(V,E) 是连通网Graph；
- T=(U,D) 是最小生成树Tree；
- V、U 是顶点集合；
- E、D 是边的集合；

2. 若从顶点 u 开始构造最小生成树，则从集合 V 中取出顶点 u 放入集合 U 中，标记顶点 v 的 <mark>visited[u]=1</mark> ，表示已经访问过。

3. 若集合 U 中顶点 ui 与集合 V-U 中的顶点 vj 之间存在边，则寻找这些边中权值最小的边，但不能构成回路，将顶点 vj 加入集合U中，将边（ui,vj）加入集合 D 中，标记 <mark>visited[vj]=1</mark>。

4. 重复 **2**，直到 U 与 V 相等，即所有顶点都访问一遍（被标记过），此时 D 中有 n-1 条边，结束。

图解如下：

![prim01.png](/images/prim01.png "Prim Algorithm"){: .align-center}

代码实现：不连通的顶点用一个很大的数表示（这里是10000）。用到的数据结构：visited[ ] 表示是否访问过，把顶点分成两个集合，对 int[ ][ ] weight 作条件判断：只有起点 i 未访问且终点 j 访问过，才考虑这个边（图中黑色加粗的边），每次更新且重置的 minWeight 找到最短边，并把终点 j 标记为已访问。核心代码如下，很好理解：

{% highlight js %}
// 1. 当前节点标记为已访问
vertexVisited[startVertex] = 1;

        // 2. 直到生成 n-1 个边停止
        for (int k = 1; k < graph.verx; k++) {
            // 对目前每个节点，找它们连着的最短节点
            for (int i =0;i<graph.verx ;i++){
                for (int j = 0;j<graph.verx;j++){
                    // 起点i是访问过的节点，终点j是没访问过的节点，条件是最短，初始是10000，第一次也照顾到了
                    if (vertexVisited[i] == 1 && vertexVisited[j] ==0 && graph.weight[i][j] < minWeight){
                        minWeight = graph.weight[i][j];
                        h1 = i;
                        h2 = j;
                    }
                }
            }
            System.out.println("第 "+k+" 轮找到的边：由 "+graph.data[h1]+" 到 "+graph.data[h2]+" ，权值为"+minWeight);
            vertexVisited[h2] = 1;
            sumWeight += minWeight;
            minWeight = 10000; // 重置minWeight
        }
{% endhighlight %}

## 六、




