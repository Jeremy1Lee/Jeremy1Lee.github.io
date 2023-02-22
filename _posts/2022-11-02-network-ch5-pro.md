---
layout: post
title: "【计网】Ch5：运输层-总结"
author: LJC
tags:
- network
date: 2022-11-02 19:50 +0800
toc: true
---

# 运输层 (Transport layer)

-------------------

## TCP 建立连接：三次握手

- ![tcpL11.png](/images/net/tcpL11.png "TCP两握手-4")

两个服务器，三次握手，三种状态，两个连接请求报文，一个普通报文。报文首部的字段值：
- 序号seq是发报文的编号
- 确认ack是针对报文的序号表示已收到
- SYN ACK 表示报文的类型：是请求还是确认

## TCP 释放连接：四次挥手

两个 释放报文段，两个 普通报文段。报文首部的字段值：


-----------------


