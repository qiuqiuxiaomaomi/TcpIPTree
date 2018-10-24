# TcpIPTree
Tcp/IP技术

![](https://i.imgur.com/7WDWnnb.jpg)

<pre>
TCP/IP协议分层
1）链路层
   通常包括操作系统中的设备驱动程序和计算机中对应的网络接口卡
2）网络层
   互联网层，在TCP/IP协议簇中包括IP协议，ICMP协议，IGMP协议
3）传输层
   传输层主要为两台主机上的应用程序提供端到端的通信，有两个互不相同的传输协议，TCP（传输控制协议）， UDP（用户数据报协议）
5）应用层
   负责特定的应用程序
</pre>

<pre>
  许多应用程序都是用TCP和UDP来传送数据，运输层协议在生成报文首部时要存入一个应用程序的标识符，TCP和UDP都用一个16bit的端口号来标识不同的应用程序，

  由于TCP,UDP,ICMP,IGMP都要向IP传送数据，因此IP必须在生成的IP首部中加入某种标识，以表明属于哪一层，为此IP在首部中存入一个长度为8bit的数据，称作协议域，6表示TCP，17标识UDP协议
</pre>

![](https://i.imgur.com/N1ko0im.jpg)

-![](https://i.imgur.com/jN7pFzR.jpg)

<pre>
IP层，网际协议
    IP路由的选择
    如果目的主机与源主机直接相连或都在一个共享网络上（以太网），那么IP数据报就直接送到目的主机上，否则主机把数据报发往一默认的路由上，由路由器来转发数据报，大多数的主机都是采用这种机制.

	在一般的体制中，IP可以从TCP,UDP,ICMP和IGMP接收数据并进行发送，或者从一个网络接口接收数据报并进行发送，IP层在内存中有一个路由表，当接收到一份数据报并进行发送时，它都要对该表搜索一次。
</pre>

<pre>
ARP地址解析协议
   ARP是IP地址到对应的硬件地址之间提供动态映射，知道主机的IP地址并不能让内核发送一帧数据给主机，内核（如以太网驱动程序）必须要知道
   目的端的硬件地址才能发送数据。
</pre>

<pre>
RARP协议
   提供硬件地址到IP地址的映射
</pre>

![](https://i.imgur.com/mvpfOXn.jpg)

<pre>
UDP 用户数据报协议
</pre>

<pre>
DNS域名系统
</pre>

![](https://i.imgur.com/dMreEZr.jpg)

TCP连接三次握手

![](https://i.imgur.com/mawg0wt.jpg)

<pre>
TCP 传送控制协议
</pre>

<pre>
TCP的超时与重传
</pre>