### 内容

```
TCP（transport control protocol，传输控制协议）是面向连接的，面向流的，提供高可靠性服务。收发两端（客户端和服务器端）都要有一一成对的socket，因此，发送端为了将多个发往接收端的包，更有效的发到对方，使用了优化方法（Nagle算法），将多次间隔较小且数据量小的数据，合并成一个大的数据块，然后进行封包。这样，接收端，就难于分辨出来了，必须提供科学的拆包机制。 即面向流的通信是无消息保护边界的.一个完整的包可能会被TCP拆分成多个包进行发送，也有可能把多个小的包封装成一个大的数据包发送，这就是所谓的TCP粘包和拆包问题
TCP的一个连接, 既有发送缓冲区, 也有接收缓冲区, 那么对于这一个连接, 既可以读数据, 也可以写数据. 这个概念叫做 全双工
```

#### 粘包和拆包
```
发生TCP粘包或拆包有很多原因，现列出常见的几点，可能不全面，欢迎补充，
1、要发送的数据大于TCP发送缓冲区剩余空间大小，将会发生拆包。
2、待发送数据大于MSS（最大报文长度），TCP在传输前将进行拆包。
3、要发送的数据小于TCP发送缓冲区的大小，TCP将多次写入缓冲区的数据一次发送出去，将会发生粘包。
4、接收数据端的应用层没有及时读取接收缓冲区中的数据，将发生粘包。
解决方案:
1、发送端给每个数据包添加包首部，首部中应该至少包含数据包的长度，这样接收端在接收到数据后，通过读取包首部的长度字段，便知道每一个数据包的实际长度了。
2、发送端将每个数据包封装为固定长度（不够的可以通过补0填充），这样接收端每次从接收缓冲区中读取固定长度的数据就自然而然的把每个数据包拆分开来。
3、可以在数据包之间设置边界，如添加特殊符号，这样，接收端通过这个边界就可以将不同的数据包拆分开。
```
#### tcp状态机
```
（1）CLOSED 状态时初始状态。
（2）LISTEN:被动打开，服务器端的 状态变为LISTEN(监听)。被动打开的概念：连接的一端的应用程序通知操作系统，希望建立一个传入的连接。这时候操作系统为连接的这一端建立一个连 接。与之对应的是主动连接：应用程序通过主动打开请求来告诉操作系统建立一个连接。
（3）SYNRECVD:服务器端收到SYN后，状态为SYN；发送SYN ACK;
（4）SYN_SENTY:应用程序发送SYN后，状态为SYN_SENT；
（5）ESTABLISHED:SYNRECVD收到ACK后，状态为ESTABLISHED； SYN_SENT在收到SYN ACK，发送ACK，状态为ESTABLISHED；
（6）CLOSE_WAIT:服务器端在收到FIN后，发送ACK，状态为CLOSE_WAIT；如果此时服务器端还有数据需要发送，那么就发送，直到数据发送完毕；此时，服务器端发送FIN，状态变为LAST_ACK;
（7）FIN_WAIT_1：应用程序端发送FIN，准备断开TCP连接；状态从ESTABLISHED——>FIN_WAIT_1；
（8）FIN_WAIT_2：应用程序端只收到服务器端得ACK信号，并没有收到FIN信号；说明服务器端还有数据传输，那么此时为半连接；
（9）TIME_WAIT:有两种方式进入 该状态：1、FIN_WAIT_1进入：此时应用程序端口收到FIN+ACK（而不是像FIN_WAIT_2那样只收到ACK，说明数据已经发送完毕）并 向服务器端口发送ACK；2、FIN_WAIT_2进入：此时应用程序端口收到了FIN，然后向服务器端发送ACK；TIME_WAIT是为了实现TCP 全双工连接的可靠性关闭，用来重发可能丢失的ACK报文；需要持续2个MSL(最大报文生存时间)：假设应用程序端口在进入TIME_WAIT后，2个 MSL时间内并没有收到FIN,说明应用程序最后发出的ACK已经收到了；否则，会在2个MSL内在此收到ACK报文；
```
#### 很多的fin_wait1状态的socket
```
主动关闭的一方发出 FIN，同时进入 FIN_WAIT1 状态，被动关闭的一方响应 ACK，从而使主动关闭的一方迁移至 FIN_WAIT2 状态，接着被动关闭的一方同样会发出 FIN，主动关闭的一方响应 ACK，同时迁移至 TIME_WAIT 状态

FIN_WAIT1 能持续多久？一般情况下，服务器间的 ACK 确认是非常快的，以至于我们凭肉眼往往观察不到 FIN_WAIT1 的存在，不过网上也有很多案例表明在某些情况下 FIN_WAIT1 会持续很长时间，从而诱发问题
内核已经考虑到了此类问题，它提供了 tcp_max_orphans 参数，用来控制 orphans 的最大值
```
[参考文档](https://blog.huoding.com/2014/11/06/383)
