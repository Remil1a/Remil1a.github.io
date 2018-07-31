---
title: Spanning Tree
date: 2018-07-17 16:38:38
categories: Cisco
tags: [H3C,Cisco,Huawei]
mathjax: true
---

# 生成树概览

在较简单的交换网络中，为了防止出现单点故障，经常会做冗余链路来提高整个交换网络的可靠性。

<!----more---->

![1](Spanning-Tree\1.png)

如图所示，若switch0和switch2下方均接入了若干PC，当网络中任意一点出现故障时，整个网络将会变得不可用，这显然是不行的。所以一般会在SWITCH0和SWITCH2之间再连一根线，这种拓扑就是有冗余链路的拓扑。这样可以防止网络中的单点故障问题。但是也随之带来了新问题。那就是网络环路。而生成树就是为了破除冗余链路带来的环路问题而被提出的。



# 802.1d

##  BPDU报文

   交换机之间通过BPDU报文的交互来计算生成树。生成树之所以叫生成树，是因为破环就是在环形的冗余链路中计算出一个无环的树形结构。

   BPDU报文一共有两种：配置BPDU（configuration BPDU）和TCN（topology change notification）

##  配置BPDU

2.1 交换网络在刚开始运行的阶段，所有交换机都会从所有的端口发送BPDU，大家都认为自己是root，随着BPDU的泛洪和收集，根据BPDU中的信息，交换机会算出一个结果。root会被选举出来，整个交换网络有且只有一个root。（选举root的过程先不提，后面再说。）在此之后由root以默认的每隔2s为周期发送BPDU，所有的非root交换机从自己的根端口收到BPDU，再从自己的指定端口产生bpdu发出去。被block的非指定端口会不断侦听链路上的bpdu，当其在一段时间内没有收到bpdu，则认为链路出现故障。开始新的收敛阶段。

> When a switch receives a configuration BPDU that contains superior information(lower bridge ID,lower path cost, and so forth),it stores the information for that port.If this BPDU is received on the root port of the switch,the switch also forwards it with an updated message to all attached LANs for which it is the designated switch. 
>
> If a switch receives a configuration BPDU that contains inferior information to that currently stored for that port,it discard the BPDU.If the switch is a designated switch for the LAN from which the inferior BPDU was received,it sends that LAN a BPDU containing the up-to-date information stored for that port.In this way,inferior information is discarded,and superior information is propagated on the network.

配置BPDU的结构如下:

```unix
1. Protocol ID:       2 bytes (0x0000 IEEE 802.1D)
 2. Version ID:        1 byte (0x00 Config & TCN / 0x02 RST / 0x03 MST / 0x04 SPT  BPDU) 
 3. BPDU Type:         1 byte (0x00 STP Config BPDU, 0x80 TCN BPDU, 0x02 RST/MST Config BPDU)
 4. Flags:             1 byte
   bits  : usage
       1 : 0 or 1 for Topology Change
       2 : 0 (unused) or 1 for Proposal in RST/MST/SPT BPDU
     3-4 : 00 (unused) or
           01 for Port Role Alternate/Backup in RST/MST/SPT BPDU
           10 for Port Role Root in RST/MST/SPT BPDU
           11 for Port Role Designated in RST/MST/SPT BPDU
       5 : 0 (unused) or 1 for Learning in RST/MST/SPT BPDU
       6 : 0 (unused) or 1 for Forwarding in RST/MST/SPT BPDU
       7 : 0 (unused) or 1 for Agreement in RST/MST/SPT BPDU
       8 : 0 or 1 for Topology Change Acknowledgement
 5. Root ID:           8 bytes (CIST Root ID in MST/SPT BPDU)
   bits  : usage
     1-4 : Root Bridge Priority
    5-16 : Root Bridge System ID Extension
   17-64 : Root Bridge MAC Address
 6. Root Path Cost:    4 bytes (CIST External Path Cost in MST/SPT BPDU)
 7. Bridge ID:         8 bytes (CIST Regional Root ID in MST/SPT BPDU)
   bits  : usage
     1-4 : Bridge Priority 
    5-16 : Bridge System ID Extension
   17-64 : Bridge MAC Address
  8. Port ID:          2 bytes
  9. Message Age:      2 bytes in 1/256 secs
 10. Max Age:          2 bytes in 1/256 secs
 11. Hello Time:       2 bytes in 1/256 secs
 12. Forward Delay:    2 bytes in 1/256 secs
 13. Version 1 Length: 1 byte (0x00 no ver 1 protocol info present. RST, MST, SPT BPDU only)
 14. Version 3 Length: 2 bytes (MST, SPT BPDU only)
 					——————摘自维基百科
```

每个字段的意义大体如下：

| 字节 | 字段                      | 描述                                                         |
| ---- | ------------------------- | ------------------------------------------------------------ |
| 2    | 协议（protocol）          | 代表上层协议（BPDU），该值总为0.                             |
| 1    | 版本（version）           | 802.1d总为0                                                  |
| 1    | 类型（Type）              | 配置BPDU为0x00、TCN BPDU为0x80                               |
| 1    | 标识（Flag）              | LSB最低有效位置位表示TC标识，MSB最高有效位置位表示TCA标识    |
| 8    | 根ID（Root ID）           | 根桥的桥ID                                                   |
| 4    | 路径开销（Path Cost）     | 到达根桥的开销                                               |
| 8    | 网桥ID（Bridge ID）       | BPDU发送桥的ID                                               |
| 2    | 端口ID（Port ID）         | BPDU发送网桥的端口ID（优先级+端口号）                        |
| 2    | 消息寿命（Message age）   | 从根网桥发出BPDU之后的秒数，每经过一个网桥都-1，所以本质上是到达根桥的跳数。 |
| 2    | 最大寿命（Max age）       | 当一段时间未收到任何BPDU，生存期到达MAX age时，网桥认为该端口连接的链路发生故障。也可理解为BPDU的最大寿命，默认20s |
| 2    | HELLO时间（Hello Time）   | 根网桥连续发出BPDU之间的时间间隔，默认2s                     |
| 2    | 转发延迟（Forward Delay） | 在侦听和学习状态停留的时间，默认15s。                        |

## TCN BPDU

3.1 TCP BPDU在网络拓扑发生变化时产生。TYPE字段为0x80，FLAG字段LSB和MSB位均置位。

   当网络拓扑发生变化的时候，最先意识到变化的交换机会从根端口发送TCN到上一层交换机，一直到根交换机，上层交换机除了接着向其上层发送TCN之外，也会回一个TCA的确认信息给前一个交换机。当根接受到TC后发送TCA回最开始的交换机并向所有交换机发送TC。交换机收到ROOT发来的TC后，会将MAC地址表的老化时间缩减为15S（一个转发延迟），这个TC会一直持续35S（20+15）。

## STP的运行

4.1 STP采用四个步骤来解决二层环路：

​	4.1.1 在一个交换网络中选举一个root bridge

​	4.1.2 在每个非根交换机上选举一个根端口（RP）

​	4.1.3 为每个segment选举一个指定端口（DP）

​	4.1.4 阻塞非指定端口

4.2 比较原则

​	4.2.1 STP需要网络设备相互交换消息来检测桥接环路，这个消息就叫**BPDU（桥协议数据单元）**即使是阻塞的端口也会不断收到BPDU。

​	4.2.2 生成树总是按照以下步骤来生成一个无环的拓扑：

- 最低的桥ID
- 到根桥最低的路径开销
- 最低的发送者桥ID
- 最低的发送者端口ID

交换机使用这四个步骤分别选举出根交换机，根端口和指定端口。并且会保存各个端口收到的最好的BPDU。每收到一个新的BPDU，都会和这个最优的BPDU进行比较。如果收到的BPDU比保存的BPDU更优，则更新。反之则不做任何操作。

> 注意：
>
> 根桥的角色是可以抢占的。桥ID中的MAC地址指的是交换机的背板MAC地址 使用show version | in Base查看。
>
> sw1-huiju#show version | in Base
> Base ethernet MAC Address       : 00:23:5E:26:D9:80
>
> 
>
> 
>
> 端口ID中的MAC是指端口的MAC地址。思科交换机上可使用show interfaces | in bia查看
>
> sw1-huiju#show interfaces | in bia
>   Hardware is EtherSVI, address is 0023.5e26.d9c0 (bia 0023.5e26.d9c0)
>   Hardware is EtherSVI, address is 0023.5e26.d9c1 (bia 0023.5e26.d9c1)
>   Hardware is EtherSVI, address is 0023.5e26.d9c2 (bia 0023.5e26.d9c2)
>   Hardware is EtherSVI, address is 0023.5e26.d9c3 (bia 0023.5e26.d9c3)
>   Hardware is EtherSVI, address is 0023.5e26.d9c4 (bia 0023.5e26.d9c4)
>   Hardware is EtherSVI, address is 0023.5e26.d9c5 (bia 0023.5e26.d9c5)
>   Hardware is EtherSVI, address is 0023.5e26.d9c6 (bia 0023.5e26.d9c6)

接下来看几组802.1d的选举实例：

![2](Spanning-Tree\2.png)

如图所示，交换机X和Y的优先级分别是1111和2222，拓扑中存在冗余链路。BPDU在经过交换后，会进行两个参数的比较来选出根桥。一是优先级。在不手动进行更改的情况下。两台交换机的优先级都为32768。所以优先级这一块是比较不出来的。这种情况下会去比较MAC地址。MAC地址是比小的。所以交换机X胜出。



​		 ![3](Spanning-Tree\3.png)



​		接下来看这组选举实例。先进行根桥的选举。根桥选举先看优先级。交换机X，Y，Z都是32768 所以比较不出来。那么就会看交换机的MAC地址。交换机Z的最小。胜出。所以交换机Z是根桥。根桥上所有的端口都是指定端口，处于转发状态。之后在非根交换机上选举根端口。



​		根端口的定义是去往根桥最小的路径开销。那么按照下面这幅图

![4](Spanning-Tree\4.png)

由图上的100Base-T和10Base-T可得，X和Y之间的链路开销会比XZ以及XY之间的大。所以交换机X和Y的port0胜出。为根端口。



接下来在XY之间的链路选举指定端口。由于交换机X的MAC地址较小。所以交换机X胜出。交换机X的port1为指定端口。那么就只剩下交换机Y的port1了。它就是NDP非指定端口。会被阻塞。



## STP端口状态

### 为什么要有STP端口状态

   由于交换网络中存在传播的延迟。拓扑变更可能会发生在网络的任何时间，任何网段。如果二层接口直接从生成树的block状态过渡到转发状态。就可能会导致临时的环路。为了缓解这种问题，在一个端口开始转发数据之前，它应该等待新的拓扑信息传播到整个交换网络中。

### 计时器

   阻塞状态到转发状态通常需要30～50秒，主要涉及到以下的计时器：

   **Hello时间：**根桥发送配置BPDU的时间间隔。默认是2s

   **转发延迟：**端口由侦听过渡到学习状态，学习状态过渡到转发状态所需要的时间。默认是15s

   **最大存活时间：** 在丢弃BPDU之前，网桥用来存储BPDU的时间。缺省为20s。如果连续20秒收不到BPDU。则进入侦听（listening）状态。

   网络中的生成树拓扑依附于根桥的计时器，根桥将BPDU中的计时器传递给第二层的所有交换机。

### STP各端口状态

 

| 状态名称   | 特性                                                |
| ---------- | --------------------------------------------------- |
| Disable    | 不收发任何报文                                      |
| Blocking   | 不接受也不转发帧，接受但不发送BPDU，不学习MAC地址。 |
| Listening  | 不接受也不转发帧，接受并发送BPDU，不学习MAC地址。   |
| Learning   | 不接受也不转发帧，接受并发送BPUD，学习MAC地址。     |
| Forwarding | 接受并转发帧，接受并发送BPDU，学习MAC地址。         |



### STP端口状态转换过程

![5](Spanning-Tree\5.png)

## STP拓扑变更

### TCN BPDU概述

**当网络拓扑出现变化时，最先发现变化的交换机将发送TCN BPDU。**

**发生以下事件时，交换机认为是拓扑变更，发送TCN BPDU**

- 对于处于转发和监听状态的端口，过渡到block状态
- 端口进入转发状态，且这个网桥已经有指定接口
- 非根桥设备在它的指定端口收到TCN BPDU

### TCN BPDU

**TCN BPDU包含3个字段，它与配置BPDU除了type字段之外的前3个字段完全相同**



### 拓扑变更的过程

最先发现拓扑变更的交换机发送TCN，上游交换机接受到TCN会立即回送TCA被置位的BPDU来作确认。在上游交换机确认这个TCN之前，最先发现拓扑变更的交换机会持续发送TCN BPDU。

接下来上游交换机将在自己的根端口产生另外的TCN BPDU，并且这整个过程会重复，一直到这个拓扑变更被root知道为止。



### 拓扑变更案例 

![6](Spanning-Tree\6.png)

如图，假设Switch A

