---
title: Spanning Tree
date: 2018-07-17 16:38:38
categories: Cisco
tags: [H3C,Cisco,Huawei]
---

# 生成树概览

在较简单的交换网络中，为了防止出现单点故障，经常会做冗余链路来提高整个交换网络的可靠性。

<!----more---->

![1](Spanning-Tree\1.png)

如图所示，若switch0和switch2下方均接入了若干PC，当网络中任意一点出现故障时，整个网络将会变得不可用，这显然是不行的。所以一般会在SWITCH0和SWITCH2之间再连一根线，这种拓扑就是有冗余链路的拓扑。这样可以防止网络中的单点故障问题。但是也随之带来了新问题。那就是网络环路。而生成树就是为了破除冗余链路带来的环路问题而被提出的。



# 802.1d

1. BPDU报文

   交换机之间通过BPDU报文的交互来计算生成树。生成树之所以叫生成树，是因为破环就是在环形的冗余链路中计算出一个无环的树形结构。

   BPDU报文一共有两种：配置BPDU（configuration BPDU）和TCN（topology change notification）

2. 配置BPDU

   1. 交换网络在刚开始运行的阶段，所有交换机都会从所有的端口发送BPDU，大家都认为自己是root，随着BPDU的泛洪和收集，根据BPDU中的信息，交换机会算出一个结果。root会被选举出来，整个交换网络有且只有一个root。（选举root的过程先不提，后面再说。）在此之后由root以默认的每隔2s为周期发送BPDU，所有的非root交换机从自己的根端口收到BPDU，再从自己的指定端口产生bpdu发出去。被block的非指定端口会不断侦听链路上的bpdu，当其在一段时间内没有收到bpdu，则认为链路出现故障。开始新的收敛阶段。

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

3. TCN BPDU

   1. TCP BPDU在网络拓扑发生变化时产生。TYPE字段为0x80，FLAG字段LSB和MSB位均置位。

