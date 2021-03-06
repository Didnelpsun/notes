# 计算机网络（Computer network）

&emsp;

## 目录

## 第一章[]()

## 第二章[]()

### &emsp;[]()

#### &emsp;&emsp;[]()

## 第三章[数据链路层](#第三章-数据链路层)

### &emsp;3.3[广播信道](#33-广播信道)

#### &emsp;&emsp;3.3.1[局域网的数据链路层](#331-局域网的数据链路层)

#### &emsp;&emsp;3.3.2[CSMA/CD协议](#332-csmacd协议)

&emsp;

## [直达底部](#回到目录)

---

&emsp;

## [第一章 ]()

&emsp;  

## [第二章 ]()

### 

#### 

## [第三章 数据链路层](#第三章数据链路层)

### [3.3 广播信道](#33广播信道)

#### &emsp;[3.3.1 局域网的数据链路层](#331局域网的数据链路层)

$\color{yellowgreen}{局域网的特点}$：网络为一个单位所拥有，且地理范围和站点数目均有限。  
$\color{yellowgreen}{局域网的优点}$：

1. 具有广播作用，一个站点访问全网。

2. 便于系统的扩展和逐渐演变，各设备的位置可变。

3. 提高了系统的可靠性、可用性和生存性。

$\color{yellowgreen}{共享信道的方法}$：

1. 静态划分信道。如频分复用等。但是代价高，不适用于局域网。

2. 动态媒体接入控制（多点接入）信道非固定分配。  

    + 随机接入，所有用户可以随机发送信息，但是如果多个用户同时发送会发生$\color{green}{碰撞}$。

    + 受控接入，用户不能随机发送消息而必须服从一定的控制。如分散控制的令牌环局域网和集中控制的多点线路$\color{green}{探询}$/$\color{green}{轮询}$。

$\color{yellowgreen}{适配器}$

又称网络接口卡（网卡）NIC。上面装有处理器和存储器。因为网络上数据率与计算机总线上数据率并不相同，所以需要缓存。同时要在操作系统上安装适配器驱动系统。  
作用：连接计算机和外界局域网。因为适配器和局域网之间的通讯是通过电缆或者双绞线进行的串行传输，而适配器和计算机的通信则是通过计算机主板上的I/O总线以并行传输，所以他也起数据串并行传输转换的作用。  
$\color{orange}{注意：}$适配器包含了数据链路层和物理层的两个层次的功能。  
适配器不会使用计算机CPU。当收到正确帧就中断通知计算机并交付协议栈中的网络层。
当计算机想发送IP数据报时就让协议栈把IP数据报下交给适配器，由它组装为帧发送给局域网。

#### &emsp;[3.3.2 CSMA/CD协议](#332CSMA/CD协议)

目的就是减少碰撞现象。意思是载波监听多点接入/碰撞检测。

$\color{yellowgreen}{要点}$：

1. 多点接入：总线型网络。

2. 载波监听：也就是监听信道。电子技术检测总线上面有没有其他计算机在发送数据。在发送中，前都不断地检测信道，这就是碰撞检测。

3. 碰撞检测：即边发送边监听。如果发现总线上信号电压变化幅度增大（出现互相叠加的现象）就是碰撞，那就控制适配器停止发送。

但是即使等到空闲也可能发生碰撞现象，因为电磁波传播有速度。$\color{green}{电磁波在1km电缆的传播时延为5微秒}$。  
使用CSMA/CD协议时一个站不可能同时发送和接受，但是必须边发送边监听信道。所以使用CSMA/DA协议的以太网不是双全工通信而是$\color{green}{双向交替通信（半双工通信）}$。  
这就是$\color{green}{发送的不确定性}$：每一个站在自己发送数据后的一段时间中，存在着遭遇碰撞的可能性。  
$\color{green}{争用期}$（$\color{green}{碰撞窗口}$）：以太网端到端的往返时间。经过争用期还没有检测到碰撞才能肯定这次发送不会产生碰撞。

$\color{purple}{例子}$：假定1km长的CSMA/CD网络的数据率为1Gbit/s 。设信号在网络上的传播速率为 200 000 km/s 。求能够使用此协议的最短帧长。
解答： I km 长的 CSMA/CD 网络的端到端传播时延. = (1km)/ (200000 km/s) = 5µs  
2τ=10µs, 在此时间内要发送(1Gbit/s) (10µs) = 10000bit  
只有经过这样 段时间后发送端才能收到碰撞的信息（如果发生碰撞的话），也才能检测
到碰撞的发生。  
因此，最短帧长为 10000 bit, 1250 字节。

$\color{yellowgreen}{截断二进制指数退避}$：用来确定碰撞后重传的时机。即退避一个随机时间。  

1. 基本退避时间为2τ，具体争用期时间为$\color{green}{51.2µs}$.对于10Mbit/s的以太网，争用期可以发送512bit的数据，即64字节，那就是争用期为512比特时间（1比特时间就是发送1比特需要的时间），也可以说是512比特。而对于100Mbit/s的以太网就是争用期可以发送5120bit的数据。

2. 从离散的整数集合[0,1,……,2^k-1]随机取出一个数r，重传的时间应该是r倍的争用期。k=Min[重传次数,10] (k在重传次数不大于10的时候等于重传次数，大于就一直等于10)

3. 当重传次数达到16次时，就丢弃帧，并报告高层。

适配器每发送一个新帧就执行一次CSMA/CD算法。无记忆功能。  

$\color{purple}{例子}$：假定在使用CSMA/CD协议的10Mbit/s的以太网中，某个站在发送数据时检测到碰撞，执行退避算法时选择了随机数r=100。试问这个站需要等待多长时间后才能再次发送数据？如果是100Mbit的以太网呢？  
解答： 对于10Mbit/s的以太网，争用期是512比特时间，现在r=100, 因此退避时间是51200比特时间。  
这个站需要等待的时间是51200/10 = 5120µs = 5.12 ms。  
对于100Mbit/s的以太网，争用期仍然是512比特时间，退避时间是 51200比特时间。  
因此，这个站需要等待的时间是 51200/100 = 512µs。

$\color{green}{长度小于64字节的帧都是由于冲突而异常终止的无效帧}$，应该被丢弃，而如果数据小于64字节，就要进行填充。

由于信号传播速度为5µs/km。所以以太网最大的端到端实验必须小于争用期的一半，即25.6µs。即长度为5km。

$\color{yellowgreen}{强化碰撞}$：当发送数据的站发生碰撞，立即停止发送数据，并发送32或者48比特的$\color{green}{人为干扰信号}$，让所有用户都知道发生了碰撞。

$\color{yellowgreen}{帧间最小间隔}$：9.6µs。使收到数据帧的站接收缓存来得及清理，做好接受下一帧的准备。

---

## [回到目录](#目录) &emsp; &emsp;[查询更多](https://github.com/Didnelpsun/notes)

### 参考

1. 计算机网络-谢希仁-<5
