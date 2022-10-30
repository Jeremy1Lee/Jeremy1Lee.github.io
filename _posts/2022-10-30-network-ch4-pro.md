---
layout: post
title: "计网 Ch4：网络层（纯享版）"
author: LJC
tags:
- network
date: 2022-10-30 14:00 +0800
toc: true
---

# 网络层 (Network layer) 的总结

网络层的三个任务：
- 向运输层提供怎样的服务？**可靠传输：虚电路/ 不可靠传输：IP数据报** （但不负责实现可靠传输，这是链路层的活）

- **网络层寻址问题**：IPv4 IPv6 地址；

- **路由选择问题【篇幅最多】：配置路由，路由选择协议等**

    - 人工配置路由，告诉路由往哪转发（小规模可以，大网络配不过来也不好维护）
    - 部署路由选择协议，根据互联网思想分成内部和外部网关协议
    - 内部网关协议：
        - RIP 协议 —— **距离向量**：两部分
            - 距离：路由表里的**跳数**表示；
            - 向量：路由表里的**下一跳**表示；
            - 建立过程：
                - ① 初始化（收敛）
                - ② 给**相邻路由**发送自己的路由表
                - ③ 根据【其他路由表 和 更新规则】，更新路由表
            - 存在固有问题：坏消息传得慢（被覆盖了）