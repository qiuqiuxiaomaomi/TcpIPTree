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


###TCP:

![](https://i.imgur.com/WcDr5sn.png)

###UDP：

![](https://i.imgur.com/PDh6Hbc.png)

###TCP头 20字节

![](https://i.imgur.com/uwU6133.png)

###UDP头 8字节

![](https://i.imgur.com/A6UueuF.png)

<pre>
TCP UDP区别

      1、TCP面向连接（如打电话要先拨号建立连接）;UDP是无连接的，即发送数据之前不需要建立连接
      2、TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且
      按序到达;UDP尽最大努力交付，即不保证可靠交付，Tcp通过校验和，重传控制，序号标识，滑
      动窗口、确认应答实现可靠传输。如丢包时的重发控制，还可以对次序乱掉的分包进行顺序控制。
      3、UDP具有较好的实时性，工作效率比TCP高，适用于对高速传输和实时性有较高的通信或广播通信。
      4.每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信
      5、TCP对系统资源要求较多，UDP对系统资源要求较少。
</pre>

<pre>
为什么UDP有时比TCP更有优势?

      UDP以其简单、传输快的优势，在越来越多场景下取代了TCP,如实时游戏。
     （1）网速的提升给UDP的稳定性提供可靠网络保障，丢包率很低，如果使用应用层重传，能够确
      保传输的可靠性。
     （2）TCP为了实现网络通信的可靠性，使用了复杂的拥塞控制算法，建立了繁琐的握手过程，由
      于TCP内置的系统协议栈中，极难对其进行改进。

         采用TCP，一旦发生丢包，TCP会将后续的包缓存起来，等前面的包重传并接收到后再继续
      发送，延时会越来越大，基于UDP对实时性要求较为严格的情况下，采用自定义重传机制，能够
      把丢包产生的延迟降到最低，尽量减少网络问题对游戏性造成影响。
</pre>

![](https://i.imgur.com/oDLCt2C.png)

<pre>
syns queue(半连接队列）
accept queue（全连接队列）

      三次握手中，在第一步server收到client的syn后，把相关信息放到半连接队列中，同时回复
      syn+ack给client（第二步）

      第三步的时候server收到client的ack，如果这时全连接队列没满，那么从半连接队列拿出相关
      信息放入到全连接队列中，否则按tcp_abort_on_overflow指示的执行

      全连接队列、半连接队列溢出这种问题很容易被忽视，但是又很关键，特别是对于一些短连接应用
      （比如Nginx、PHP，当然他们也是支持长连接的）更容易爆发。 一旦溢出，从cpu、线程状态看起
      来都比较正常，但是压力上不去，在client看来rt也比较高（rt=网络+排队+真正服务时间），
      但是从server日志记录的真正服务时间来看rt又很短。
</pre>

<pre>
TCP中的delay ack

     delay ack是指收到包后不立即ack，而是等一小会（比如40毫秒）看看，如果这40毫秒以内正
     好有一个包（比如上面的http response）发给client，那么我这个ack包就跟着发过去（顺风
     车，http reponse包不需要增加任何大小），这样节省了资源。 当然如果超过这个时间还没有包
     发给client（比如nginx处理需要40毫秒以上），那么这个ack也要发给client了（即使为空，
     要不client以为丢包了，又要重发http request，划不来）
</pre>

![](https://i.imgur.com/5UagTBe.png)

