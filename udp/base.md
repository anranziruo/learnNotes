### 内容

```
UDP（User Data Protocol，用户数据报协议）是一个简单的面向数据报的运输层协议。它不提供可靠性，只是把应用程序传给IP层的数据报发送出去，但是不能保证它们能到达目的地。由于UDP在传输数据报前不用再客户和服务器之间建立一个连接，且没有超时重发等机制，所以传输速度很快。
特点:
无连接：知道对端的IP和端口号就直接进行传输, 不需要建立连接。
不可靠：没有确认机制, 没有重传机制; 如果因为网络故障该段无法发到对方, UDP协议层也不会给应用层返回任何错误信息。
面向数据报：不能够灵活的控制读写数据的次数和数量，应用层交给UDP多长的报文, UDP原样发送, 既不会拆分, 也不会合并。数据收不够灵活，但是能够明确区分两个数据包，避免粘包问题。
有单播，多播，广播的功能
```
[参考文档](https://blog.csdn.net/xiaobangkuaipao/article/details/76793702)

