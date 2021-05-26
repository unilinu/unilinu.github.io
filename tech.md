## [RDMA](https://zhuanlan.zhihu.com/p/93727843)

- [基本概念](https://zhuanlan.zhihu.com/p/55142557)
- [深入浅出全面解析RDMA](https://tjcug.github.io/blog/2018/06/04/%E6%B7%B1%E5%85%A5%E6%B5%85%E5%87%BA%E5%85%A8%E9%9D%A2%E8%A7%A3%E6%9E%90RDMA/)

### 基本术语

1. Fabric：支持RDMA的局域网

   `A local-area RDMA network is usually referred to as a fabric.`

2. CA(Channel Adapter)

   `A channel adapter is the hardware component that connects a system to the fabric.`

3. Verbs

   `大致可以理解为访问RDMA硬件的“一组标准动作”`

4. Memory Registration(MR) | 内存注册
   - 创建两个key (local和remote)指向需要操作的内存区域
   - 注册的keys是数据传输请求的一部分

5. Queues | 队列

   RDMA一共支持三种队列，发送队列(SQ)和接收队列(RQ)，完成队列(CQ)。其中，SQ和RQ通常成对创建，被称为Queue Pairs(QP)。

6. RDMA数据传输

   - RDMA Read | RDMA读操作 (Pull)
   - RDMA Write | RDMA写操作 (Push)
   - RDMA Write with Immediate Data | 支持立即数的RDMA写操作

### 主要特点

传统的TCP/IP技术在数据包处理过程中，要经过操作系统及其他软件层，需要占用大量的服务器资源和内存总线带宽，数据在系统内存、处理器缓存和网络控制器缓存之间来回进行复制移动，给服务器的CPU和内存造成了沉重负担。尤其是网络带宽、处理器速度与内存带宽三者的严重"不匹配性"，更加剧了网络延迟效应。

RDMA是一种新的直接内存访问技术，RDMA让计算机可以直接存取其他计算机的内存，而不需要经过处理器的处理。RDMA将数据从一个系统快速移动到远程系统的内存中，而不对操作系统造成任何影响。

![RDMA v.s. 传统TCP/IP](https://tjcug.github.io/blog/images/pasted-62.png)

在实现上，RDMA实际上是一种智能网卡与软件架构充分优化的远端内存直接高速访问技术，通过将RDMA协议固化于硬件(即网卡)上，以及支持Zero-copy和Kernel bypass这两种途径来达到其高性能的远程直接数据存取的目标。 使用RDMA的优势如下：

- 零拷贝(Zero-copy) ：应用程序能够直接执行数据传输，在不涉及到网络软件栈的情况下。数据能够被直接发送到缓冲区或者能够直接从缓冲区里接收，而不需要被复制到网络层。
- 内核旁路(Kernel bypass) - 应用程序可以直接在用户态执行数据传输，不需要在内核态与用户态之间做上下文切换。
- 不需要CPU干预(No CPU involvement)：应用程序可以访问远程主机内存而不消耗远程主机中的任何CPU。远程主机内存能够被读取而不需要远程主机上的进程（或CPU)参与。远程主机的CPU的缓存(cache)不会被访问的内存内容所填充。
- 消息基于事务(Message based transactions) - 数据被处理为离散消息而不是流，消除了应用程序将流切割为不同消息/事务的需求。
- 支持分散/聚合条目(Scatter/gather entries support)：RDMA原生态支持分散/聚合。也就是说，读取多个内存缓冲区然后作为一个流发出去或者接收一个流然后写入到多个内存缓冲区里去。

在具体的远程内存读写中，RDMA操作用于读写操作的远程虚拟内存地址包含在RDMA消息中传送，远程应用程序要做的只是在其本地网卡中注册相应的内存缓冲区。远程节点的CPU除在连接建立、注册调用等之外，在整个RDMA数据传输过程中并不提供服务，因此没有带来任何负载。

