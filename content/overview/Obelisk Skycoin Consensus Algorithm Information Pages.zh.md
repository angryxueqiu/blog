+++
title = "方尖碑：Skycoin共识算法| 信息页"
tags = [
    "Overview",
    "Consensus",
    "Obelisk",
]
bounty = 10
date = "2017-09-09"
categories = [
    "Overview",
]
author = "johnstuartmill"
+++

![Obelisk The Skycoin Consensus Algorithm](/img/obelisk-the-skycoin-consensus-algorithm.png)

<!-- MarkdownTOC autolink="true" bracket="round" -->

- [共识亮点](#consensus-highlights)
    - [为什么使用共识?](#为什使用共识?)
    - [高可扩展性和低能源消耗](#high-scalability-and-low-energy-consumption)
    - [协同攻击下的稳健性](#robust-to-coordinated-attacks)
    - [“51％攻击”](#the-%E2%80%9C51-percent-attack%E2%80%9D)
    - [隐藏的IP地址](#hidden-ip-addresses)
    - [独立的时钟同步性](#independence-of-clock-synchronization)
    - [两种类型的节点：共识与块制作](#two-type-of-nodes-consensus-and-block-making)
- [Skycoin共识算法的工作原理](#how-skycoin-consensus-algorithm-works)
- [参考](#references)

<!-- /MarkdownTOC -->


## Consensus highlights
共识亮点

### 为什使用共识?

Skycoin共识算法（“方尖碑”）同步Skycoin在所有的网络节点板块链的状态。这容许一致的计算，即计算用于给定的公钥（或地址）硬币余额时, 在该执行的计算的每个节点都会有相同的值。

### 高可扩展性和低能源消耗

按照设计，该算法是一个可扩展的计算，廉价的另类工作证明(proof-of-work)，因此共识算法和块制作可在价格低，能耗低预算的硬件上运行，从而使加密货币网络没有这么容易给集中起来（即，经由公众负担得起的的节点）。

### 协同攻击下的稳健性

我们的共识算法（I）收敛速度快，（二）需要最少的网络流量，以及（iii）可承受恶意节点的组织良好的网络大规模协同攻击。该算法非迭代，速度快，可以稀疏网络上的唯一的最近邻连接（例如在网状网络上）上运行，并在循环中的连通图的存在效果很好（不要求DAG型连接）。

### “51％攻击”

在有限的意义上说，算法的基本版本会受到这种攻击。具体而言，当迫主要广播是改性或恶意节点和UTXO兼容协议兼容的候选块，那最后该块会赢得共识。然而，任何一种不遵守块是由（未修改）算法块都会在参加共识庭审前给拿掉。

共识节点可以选择性地利用网络的信托概念在这样一种方式，使来自未知节点（即由不可信的公共密钥签名的）的消息被忽略。

在网络的信托模式启用的情况下，一个非常大的一些恶意共识节如果开始以（a）安排块链叉或（b）扰乱共识的过程攻击的话, 这一些攻击收效甚微，除非绝大多数的网络的信托点议员在不知不觉之下把他们的他们的本地列表包括了和信任了那些恶意节点到节点。

### 隐藏IP地址

该节点的地址由他们的密码公钥秉所分发。节点的IP地址，只会被它直接连接的节点知道。

### 时钟同步的独立性

该算法不使用“挂钟”（即日历日期/时间）。取而代之的是，从它们验证共识和板块链相关消息提取出的块的序列号被用于计算节点的内部时间。这可以被非正式地称为“块时钟”。

### 两种类型的节点：共识与块制作

共识节点从一个或多个块制作节点接收其输入。共识和块制造的算法是独立的，但它们都在相同的数据结构下进行操作。我们提到块制造有助于理解共识算法以及它如何与系统的其他部分整合。

这两种节点同时执行验证著作权和欺诈传入的日期的检测。欺诈或无效的消息会被检测，丢弃，绝不会传播; 从事可疑活动对等节点会被断开，它们的公钥会被禁止。


## Skycoin共识算法的工作原理
仅用于阐述的目的，下面的描述假设为：（i）每一个节点既是块制作者和共识的参与者，以及（ii）由非可信的节点产生的共识相关的消息可以被接受，即，没有基网络的信托模式过滤。一个完整的实现（即没有这些简化的假设）将在Skycoin GitHub的仓库中。模拟的结果和共识试验的详细图解示例，，请参见[\[1\]](#references) 。信任关系的网络的模拟，在使用不同的Skycoin算法之下，可参见[\[2\]](#references) 。 Skycoin共识算法的描述如下。


1.  *块制作*. 每个块制作收集新的交易，以UTXO验证所需的序列号，把交易打包兼容到一个新的模块，并把这个块广播到网络。

2.  *采集块*.每个共识节点把由块制定者生成的块收集，并将它们放入由块容器（和板块链分开）之后以块序列号标签。

3.  *选择中奖块*.每个共识节点，在接收到足够大的号码[^1]的候选块的或当满足其他条件，通过找出最多块制作者所做出来的块。中间的联系由确定性去因决定。这样的块会被标记为“本地赢家” [^2]和附加到本地板块链。对应于本地赢家的块序列号的键-值对会被删除，从而回收储存。当地赢家的哈希密码是广播/公布。

4.   *验证步骤* 。每个节点储存了对其他节点初所报导的本地获奖者统计数据。当本地获奖者已经通过全部或大部分的节点[^3]，节点就会确定整体赢家的序列号，。如果全球赢家就正正是当地的赢家，则节点继续像上述一运作样。否则，根据外部数据和本地日志节点会做出以下之一个决定，:（a）重新同步本身与网络或（b）退出共识和/或块制作 (c)保持其板块链，并请求紧急停止。

[^1]: 这是在算法之中可以自由配置的参数。
[^2]: 在某些理想的条件下，局部优胜者（对于给定的块序列号）都是相同的，即包括相同的一组交易。差别是由于网络等待时间，交易的频率高，序列之外的消息传递，消息丢失，故障或恶意节点等
[^3]: 这个数字可以被确定，例如，递归地通过询问可信任节点报告他们的信任节点的公钥。

## References

\[1\] johnstuartmill et al. A Distributed Consensus Algorithm for
Cryptocurrency Networks.
<https://github.com/skycoin/whitepapers/blob/master/whitepaper_skycoin_consensus_v01_jsm.pdf>
2016

\[2\] Houwu Chen and Jiwu Shu. Sky: an Opinion Dynamics Framework and Model
for Consensus over P2P Network.
<https://github.com/skycoin/whitepapers/blob/master/Sky-%20Opinion%20Dynamics%20Based%20Consensus%20for%20P2P%20Network%20with%20Trust%20Relationships.pdf>
201?

*阅读更多：*

* *[Skycoin Consensus Algorithm Whitepapers](https://www.skycoin.net/whitepapers)*
* *[Obelisk The Skycoin Consensus Algorithm](/statement/obelisk-skycoin-consensus-algorithm/)*
