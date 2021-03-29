

# 概述

## 网络的网络

**网络**把**主机**连接起来，而**互连网**（internet）是把多种不同的**网络**连接起来，因此互连网是网络的网络。而互联网（Internet）是全球范围的互连网。

> 主机相连构成网络，网络相连构成互连网
>
> 其中，**互联网**是全球的**互联网**

计算机网络是一个将`分散的`(地理位置不同的)、具有`独立`功能的`计算机系统`，通过`通信设备`(路由等)与`线路`(光纤等)连接起来，由功能完善的`软件`实现`资源共享`和`信息传递`的系统。

计算机网络是一些`互联的、自治的计算机系统的集合`

## 计算机网络的功能

### （1）数据通信

- 数据通信是计算机网络最基本和最重要的功能，实现计算机之间的各种信息传输，并将分散在不同地理位置的**计算机联系起来**，进行统一的**调配、控制和管理**

### （2）资源共享

- 资源共享可以是**软件**共享、**数据**共享和**硬件**共享
- 计算机网络中的资源互通有无，分工协作，从而极大地提高硬件资源、软件资源和数据资源的利用率

### （3）分布式处理

- 当计算机网络中的**某个计算机系统负荷过重**时，可以**将其处理的某个复杂任务分配给网络中的其它计算机系统**，让它帮你处理，从而利用**空闲计算机资源以提高整个系统的利用率**

### （4）提高可靠性

- 计算机网络中的各台计算机可以**通过网络互为替代机**，当一台计算机崩了，可以让另一台计算机来完成它的工作

### （5）负载均衡

- 将工作任务均衡的分配给计算机网络中的各台计算机













## ISP

**互联网服务提供商** （ISP）可以从**互联网管理机构**获得许多 IP 地址，同时拥有**通信线路**以及路由器等联网设备，个人或机构向 ISP **缴纳一定的费用就可以接入互联网**。目前的**互联网是一种多层次 ISP 结构**，ISP 根据覆盖面积的大小分为**第一层 ISP**、**区域 ISP** 和**接入 ISP**。	  **互联网交换点**（ IXP） 允许两个 ISP 直接相连而不用经过第三个 ISP。

![img](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/3be42601-9d33-4d29-8358-a9d16453af93.png)

## 主机之间的通信方式

#### ①、客户-服务器（C/S）：客户是服务的请求方，服务器是服务的提供方。

#### ②、对等（P2P）：不区分客户和服务器。



## 计算机网络分类 之 按交换技术分类 【电路交换与分组交换】

### ①、电路交换

**电路交换**用于**电话通信系统**，两个用户要**通信之前**需要建立一条**专用的物理链路**，并且在整个通信过程中**始终占用**该链路，包括建立连接（占用通信资源）、传输数据（一直占用通信资源）和断开连接（释放通信资源）三个阶段。由于通信的过程中不可能一直在使用传输线路，因此电路交换对线路的**利用率很低**，往往**不到 10%**。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200227161255388.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

### ②、报文交换

每个分组都有**首部和尾部**，包含了**源地址、目的地址、校验码等辅助信息****，然后封装成**报文**、这个报文传送到**相邻结点**，**全部存储**后，再**转发给下一个结点**，重复这一过程**直到到达目的结点**，每个报文可以单独选择到达目的结点的路径。可以实现在**同一个传输线路**上**同时**传输**多个分组**互相不会影响，也就是说分组交换**不需要占用传输线路**。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200227162056198.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

### ③、分组交换

将数据分成**较短的固定长度的数据块**，在每个数据块中加上**目的地址、源地址等辅助信息组成分组（包）**，以`储存-转发方式`传输（与报文交换一样）。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200227162714509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200227134523931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

## 时延

- 指数据（**一个报文或分组**）从网络（或链路）的**一端传送到另一端所需要的总时间**，它由4部分构成
- 总时延 = 排队时延 + 处理时延 + 传输时延 + 传播时延

![img](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/4b2ae78c-e254-44df-9e37-578e2f2bef52.jpg)

### 1. 排队时延

分组在**路由器**的**输入队列和输出队列**中排队等待的时间，取决于**网络当前的通信量**。

### 2. 处理时延

**主机或路由器**收到分组时进行处理所需要的时间，例如**分析首部**、**从分组中提取数据**、**进行差错检验**或**查找适当的路由**等。

### 3. 传输/发送时延

**主机或路由器**传输数据帧所需要的时间。

![image-20210328155653027](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210328155701.png)

### 4. 传播时延

电磁波在信道中传播所需要花费的时间，**电磁波传播的速度接近光速**。

![image-20210328155818856](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210328155819.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200227210903595.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

### 5. 时延带宽积

- **指发送端发送的第一个比特即将到达终点时，发送端已经发出了多少个比特。**
- 因此又称为**以比特为单位的链路长度**
- `时延带宽积 = 传播时延×信道带宽`

### 6. 往返时延（RTT）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200227210322762.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

### 7. 信道利用率

- 指出某一信道有百分之多少的时间是有数据通过的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200227211320843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)























