---
layout: archive-net
permalink: /network/
title: "计算机网络索引"
type: try
---

# （一）五层协议索引

计网架构五层协议的每层在网络中起到的**作用**，以及各层实际的**作用范围**，注意，越高层作用范围也越大。

![nes04.png](/images/net/nes04.png "五层协议"){: .align-center}

[一、体系架构](https://jeremy1lee.github.io/2022/10/20/network-ch1/)

-----------------------
[二、物理层](https://jeremy1lee.github.io/2022/10/21/network-ch2/)

---------------------
**三、数据链路层**

- [数据链路层-1](https://jeremy1lee.github.io/2022/10/21/network-ch3-1/)

- [数据链路层-2](https://jeremy1lee.github.io/2022/10/24/network-ch3-2/)

- [数据链路层-总结](https://jeremy1lee.github.io/2022/10/22/network-ch3-pro/)

- MAC 地址的格式：【AA-BB-CC-DD-EE-FF】

----------------
**四、网络层**
- [网络层-1](https://jeremy1lee.github.io/2022/10/27/network-ch4-1/)

- [网络层-2](https://jeremy1lee.github.io/2022/10/29/network-ch4-2/)

- [网络层-总结](https://jeremy1lee.github.io/2022/10/30/network-ch4-pro/)

- IPv4 地址的格式：【a.b.c.d】

ICMP：封装特殊的报文（如差错等）来帮助提高网际层IP协议的执行效率

[五、运输层](https://jeremy1lee.github.io/2022/10/24/network-ch3-2/)

[六、应用层](https://jeremy1lee.github.io/2022/10/24/network-ch3-2/)

-----------------
# （二）重要器件索引

## 物理层：

中继器

[集线器（Hub）](https://jeremy1lee.github.io/2022/10/24/network-ch3-2/#81-%E9%9B%86%E7%BA%BF%E5%99%A8%E6%80%BB%E7%BA%BF%E7%BD%91%E7%89%A9%E7%90%86%E5%B1%82)：什么都不能隔离；总线

## 数据链路层：

网桥（两端口的交换机），交换机是多端口网桥

[交换机（Switch）](https://jeremy1lee.github.io/2022/10/24/network-ch3-2/#82-%E4%BB%A5%E5%A4%AA%E7%BD%91%E4%BA%A4%E6%8D%A2%E6%9C%BA-%E5%9C%A8%E6%95%B0%E6%8D%AE%E9%93%BE%E8%B7%AF%E5%B1%82%E5%B7%A5%E4%BD%9C)：隔离冲突域。

## 网络层：

[路由器（Router）](https://jeremy1lee.github.io/2022/10/24/network-ch3-2/#112-%E8%B7%AF%E7%94%B1%E5%99%A8%E5%88%86%E5%89%B2%E5%B9%BF%E6%92%AD%E5%9F%9F)：即隔离冲突域，也隔离广播域。

