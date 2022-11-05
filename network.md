---
layout: archive-net
permalink: /network/
title: "计算机网络索引"
type: try
---

# （一）五层协议索引

计网架构五层协议的每层在网络中起到的**作用**，以及各层实际的**作用范围**，注意，越高层作用范围也越大。

![nes11.png](/images/net/nes11.png "五层协议"){: .align-center}

------

### 一、计算机网络的体系架构

[体系架构](https://jeremy1lee.github.io/2022/10/20/network-ch1/)

-----------------------

### 二、物理层

[物理层](https://jeremy1lee.github.io/2022/10/21/network-ch2/)

---------------------
### 三、数据链路层

- [数据链路层 - Part 1](https://jeremy1lee.github.io/2022/10/21/network-ch3-1/)

- [数据链路层 - Part 2](https://jeremy1lee.github.io/2022/10/24/network-ch3-2/)

- [数据链路层-总结](https://jeremy1lee.github.io/2022/10/22/network-ch3-pro/)


停等协议，GBN（回退N帧）协议，选择重传协议


<br/>

MAC 地址的格式：【AA-BB-CC-DD-EE-FF】

----------------
### 四、网络层

- [网络层 - Part 1](https://jeremy1lee.github.io/2022/10/27/network-ch4-1/)

- [网络层 - Part 2](https://jeremy1lee.github.io/2022/10/29/network-ch4-2/)

- [网络层-总结](https://jeremy1lee.github.io/2022/10/30/network-ch4-pro/)

<br/>

IPv4 地址的格式：【a.b.c.d】

ICMP：封装特殊的报文（如差错等）来帮助提高网际层IP协议的执行效率

### 五、运输层

- [运输层 - Part 1](https://jeremy1lee.github.io/2022/10/31/network-ch5-1/)

- [运输层 - Part 2](https://jeremy1lee.github.io/2022/11/02/network-ch5-2/)

- [运输层 - 总结](https://jeremy1lee.github.io/2022/11/02/network-ch5-pro/)

------

### 六、应用层

[应用层](https://jeremy1lee.github.io/2022/11/03/network-ch6/)

-----------------
# （二）重要概念索引

### 物理层：

- 中继器

- [集线器（Hub）](https://jeremy1lee.github.io/2022/10/24/network-ch3-2/#81-%E9%9B%86%E7%BA%BF%E5%99%A8%E6%80%BB%E7%BA%BF%E7%BD%91%E7%89%A9%E7%90%86%E5%B1%82)：什么都不能隔离；总线

### 数据链路层：

- **网桥**：是两端口的交换机，交换机是多端口的网桥；

- [交换机（Switch）](https://jeremy1lee.github.io/2022/10/24/network-ch3-2/#82-%E4%BB%A5%E5%A4%AA%E7%BD%91%E4%BA%A4%E6%8D%A2%E6%9C%BA-%E5%9C%A8%E6%95%B0%E6%8D%AE%E9%93%BE%E8%B7%AF%E5%B1%82%E5%B7%A5%E4%BD%9C)：隔离冲突域。

### 网络层：

- [路由器（Router）](https://jeremy1lee.github.io/2022/10/24/network-ch3-2/#112-%E8%B7%AF%E7%94%B1%E5%99%A8%E5%88%86%E5%89%B2%E5%B9%BF%E6%92%AD%E5%9F%9F)：即隔离冲突域，也隔离广播域。


### 运输层：

- [端到端](https://jeremy1lee.github.io/2022/10/31/network-ch5-1/#%E7%AB%AF%E5%88%B0%E7%AB%AF-%E5%92%8C-%E7%82%B9%E5%88%B0%E7%82%B9)


-----------------
# （三）重要算法/机制索引

#### 滑动窗口算法

滑动窗口算法是一种思想，它在计网中的应用有：

- 数据链路层：实现可靠传输的 [回退N帧/GBN协议](https://jeremy1lee.github.io/2022/10/21/network-ch3-1/#42-%E5%8F%AF%E9%9D%A0%E4%BC%A0%E8%BE%93%E7%AC%AC%E4%BA%8C%E7%A7%8D%E5%AE%9E%E7%8E%B0%E5%9B%9E%E9%80%80-n-%E5%B8%A7--%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E7%AE%97%E6%B3%95)
    
- 网络层：TCP的流量控制基于 [滑动窗口协议](https://jeremy1lee.github.io/2022/10/31/network-ch5-1/#4-tcp-%E7%9A%84%E6%B5%81%E9%87%8F%E6%8E%A7%E5%88%B6)




-----------------
# （四）一些你问我答


