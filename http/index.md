## 内容

### url完整的一次请求
```
对www.baidu.com这个网址进行DNS域名解析，得到对应的IP地址
根据这个IP，找到对应的服务器，发起TCP的三次握手
建立TCP连接后发起HTTP请求
服务器响应HTTP请求，浏览器得到html代码
浏览器解析html代码，并请求html代码中的资源（如js、css、图片等）（先得到html代码，才能去找这些资源）
浏览器对页面进行渲染呈现给用户
服务器关闭关闭TCP连接
```

### TCP三次握手
```
第一次握手：建立连接。客户端发送连接请求报文段，将SYN位置为1，Sequence Number为x；然后，客户端进入SYN_SEND状态，等待服务器的确认；
第二次握手：服务器收到SYN报文段。服务器收到客户端的SYN报文段，需要对这个SYN报文段进行确认，设置Acknowledgment Number为x+1(Sequence Number+1)；同时，自己自己还要发送SYN请求信息，将SYN位置为1，Sequence Number为y；服务器端将上述所有信息放到一个报文段（即SYN+ACK报文段）中，一并发送给客户端，此时服务器进入SYN_RECV状态；
第三次握手：客户端收到服务器的SYN+ACK报文段。然后将Acknowledgment Number设置为y+1，向服务器发送ACK报文段，这个报文段发送完毕以后，客户端和服务器端都进入ESTABLISHED状态，完成TCP三次握手。
```
### TCP四次挥手
```
第一次分手：主机1（可以使客户端，也可以是服务器端），设置Sequence Number，向主机2发送一个FIN报文段；此时，主机1进入FIN_WAIT_1状态；这表示主机1没有数据要发送给主机2了；
第二次分手：主机2收到了主机1发送的FIN报文段，向主机1回一个ACK报文段，Acknowledgment Number为Sequence Number加1；主机1进入FIN_WAIT_2状态；主机2告诉主机1，我“同意”你的关闭请求；
第三次分手：主机2向主机1发送FIN报文段，请求关闭连接，同时主机2进入LAST_ACK状态；
第四次分手：主机1收到主机2发送的FIN报文段，向主机2发送ACK报文段，然后主机1进入TIME_WAIT状态；主机2收到主机1的ACK报文段以后，就关闭连接；此时，主机1等待2MSL后依然没有收到回复，则证明Server端已正常关闭，那好，主机1也可以关闭连接了。
为什么要四次分手
TCP协议是一种面向连接的、可靠的、基于字节流的运输层通信协议。TCP是全双工模式，这就意味着，当主机1发出FIN报文段时，只是表示主机1已经没有数据要发送了，主机1告诉主机2，它的数据已经全部发送完毕了；但是，这个时候主机1还是可以接受来自主机2的数据；当主机2返回ACK报文段时，表示它已经知道主机1没有数据发送了，但是主机2还是可以发送数据到主机1的；当主机2也发送了FIN报文段时，这个时候就表示主机2也没有数据要发送了，就会告诉主机1，我也没有数据要发送了，之后彼此就会愉快的中断这次TCP连接。
```

### http请求头
```
请求头部为请求报文添加了一些附加信息，由“名/值”对组成，每行一对，名和值之间使用冒号分隔。
常见请求头如下：
1、Accept，浏览器端能够处理的内容类型。
例如：  Accept: text/html  代表浏览器可以接受服务器回发的类型为 text/html  也就是我们常说的html文档。如果服务器无法返回text/html类型的数据，服务器应该返回一个406错误(non acceptable)。通配符 * 代表任意类型，例如  Accept: */*  代表浏览器可以处理所有类型,(一般浏览器发给服务器都是发这个)。
 2、Accept-Encoding， 浏览器能够处理的的压缩编码。通常指定压缩方法，是否支持压缩，支持什么压缩方法（gzip，deflate），（注意：这不是指字符编码）。
例如： Accept-Encoding: zh-CN,zh;q=0.8
 3、Accept-Language， 浏览器当前设置的语言。 
语言跟字符集的区别：中文是语言，中文有多种字符集，比如big5，gb2312，gbk等等；例如： Accept-Language: en-us
4、Accept_Charset:：浏览器能够显示的字符集
5、Connection：浏览器与服务器的连接类型
例如：Connection: keep-alive   当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接。
例如： Connection: close  代表一个Request完成后，客户端和服务器之间用于传输HTTP数据的TCP连接会关闭。
当客户端再次发送Request，需要重新建立TCP连接。
 6、Host，发送请求的页面的域名。（发送请求时，该报头域是必需的），请求报头域主要用于指定被请求资源的Internet主机和端口号，它通常从HTTP URL中提取出来的。
例如: 我们在浏览器中输入：http://www.hzau.edu.cn，浏览器发送的请求消息中，就会包含Host请求报头域，如下：
Host：www.hzau.edu.cn，此处使用缺省端口号80，若指定了端口号，则变成：Host：指定端口号。
 7、Referer，发送请求的页面的URI。当浏览器向web服务器发送请求的时候，一般会带上Referer，告诉服务器我是从哪个页面链接过来的，服务器借此可以获得一些信息用于处理。
比如从我主页上链接到一个朋友那里，他的服务器就能够从HTTP Referer中统计出每天有多少用户点击我主页上的链接访问他的网站。
8、User-Agent，浏览器的用户代理字符串。告诉HTTP服务器， 客户端使用的操作系统和浏览器的名称和版本。
我们上网登陆论坛的时候，往往会看到一些欢迎信息，其中列出了你的操作系统的名称和版本，你所使用的浏览器的名称和版本，这往往让很多人感到很神奇，实际上，服务器应用程序就是从User-Agent这个请求报头域中获取到这些信息User-Agent请求报头域允许客户端将它的操作系统、浏览器和其它属性告诉服务器。
例如： User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0; CIBA;
 .NET CLR 2.0.50727; .NET CLR 3.0.4506.2152; .NET CLR 3.5.30729; .NET4.0C; InfoPath.2; .NET4.0E)
9、Cookie，用来存储一些用户信息以便让服务器辨别用户身份的（大多数需要登录的网站上面会比较常见）。
比如cookie会存储一些用户的用户名和密码，当用户登录后就会在客户端产生一个cookie来存储相关信息，这样浏览器通过读取cookie的信息去服务器上验证并通过后会判定你是合法用户，从而允许查看相应网页。当然cookie里面的数据不仅仅是上述范围，还有很多信息可以存储是cookie里面，比如sessionid等。
 8、Cache-Control，指明当前资源的有效期，控制浏览器是否直接从浏览器缓存取数据，还是重新发请求到服务器获取数据。
我们网页的缓存控制是由HTTP头中的“Cache-control”来实现的，常见值有private、no-cache、max-age、must-revalidate等，默认为private。这几种值的作用是根据重新查看某一页面时不同的方式来区分的：

```

### http响应头
```
Access-Control-Allow-Origin	指定哪些网站可以跨域资源共享	Access-Control-Allow-Origin:*
Accept-Patch	指定服务器所支持的文档补丁格式	Accept-Patch:text/exa,ple;charset=utf-8
Accept-Ranges	服务器支持的内容范围	Accept-Ranges:bytes
Age	响应对象在代理缓存中存在的时间，以秒为单位	Age:12
Allow	对于资源的有效动作	Allow:GET,POST,HEAD
Cache-control	告知客户端缓存机制，表示是否可缓存
或缓存有效时间以秒为单位	Cache-control:no-cache
Connection	针对该链接的所有预期的选项	Connection:close
Content-Disposition	对已知MIME类型资源的描述，浏览器可以根据此响应决定动作，如下载资源或打开	Content-Disposition:attachment;filename="fname.text"
Content-Encoding	响应资源使用的编码类型	Content-Encoding:gzip
Content-language	响应内容使用的语言	Content-language:zh-cn
Content-Length	响应消息体的长度，使用八进制表示	Content-Length:348
Content-Location	返回数据的一个候选位置	Content-Location:/index.htm
Content-MD5	响应内容的MD5散列值以Base64方式编码	Content-MD5:IDKOiSsGsvjkKJHkjKbg
Content-Range	如果响应的是部分消息，则表示属于完整消息的哪个部分	Content-Range:bytes12020-47021/47022
Content-Type	当前内容的MIME类型	Content-Type:text/html;charset=utf-8
Date	此条消息被发送时的日期和时间(以RFC 7231中定义的"HTTP日期"格式来表示)	Date:Wed,18 Jul 2018 21:01:33 GTM
Etag	对于某个资源的特定版本的一个标识符，
通常是一个消息散列	Etag:977823cd9080da09vsd89kj923jhkb8df8
Expires	指定一个日期/时间，超过该时间此回应过期	Expires:Wed,18 Jul 2018 21:01:33 GTM
Last-Modified	请求对象的最后修改时间
(以RFC 7231中定义的"HTTP日期"格式来表示)	Last-Modified:Wed,18 Jul 2018 21:01:33 GTM
Link	用来表示与另外一个资之间的类型关系，
此类型关系是在RFC 5988中定义的	Link:rel="alternate"
Location	用于重定向或者在创建了某个新资源时使用	Location:http://www.baidu.com
P3P	P3P策略的设置	P3P:CP="This is not a P3Ppolicy!"
Pragaa	效果并不确定，这些响应头可能在请求/回应链
中的不同时候产生不同的效果	Pragaa:no-cache
Proxy-Authenticate	要求在访问代理是提供身份认证信息	Proxy-Authenticate:Basic
Public-Key-Pins	用于防止中间攻击，申明网站认证中
传输层安全协议的证书散列值	Public-Key-Pins:max-age=259200;pin-sha256="… ..."
Refresh	用于重定向或者当一个新的资源被创建时
默认在5秒后刷新重定向	Refresh:5;url=http://www.baidu.com
Retry-After	如果某个实体临时不可用，那么此协议用于告知用户
端稍后重试，其值可以是特定的时间段(以秒为单位)
或者是一个超文本传输协议日期	Retry-After:120/Wed,18 Jul 2018 21:01:33 GTM
Server	服务器名称	Server:nginx/1.6.3
Set-Cookie	设置HTTP Cookie,cookie被设置在请求的服务端域名下	Set-Cookie:user_name=garrett;user_id=001
Status	通用网管接口响应字段，用来说明当前HTTP链接状态	Status:200 ok
Trailer	说明传输中分块编码的编码信息	Trailer:Max-Forwards
Transfer-Encoding	表示实体传输给用户的编码形式，包括：chunked,
compress,deflate,gzip,identify等	Transfer-Encoding:chunked
Upgrade	要求用户升级到另外一个高版本的协议	Upgrade:HTTP/2.0,SHTTP/1.3,IRC/6.9,RTA/X11
Vary	告知下游的代理服务器如何对之后的请求协议头进行
匹配，以决定是否可使用已缓存的响应内容而不是
重新从原服务器请求新的内容	Vary:*
Vis	告知代理服务器的客户端，当前的响应式通过什么途径发送的	Vis:1.0 FRED, 1.1 baidu.com

状态码:
http状态码可以让我很方便的了解到请求的所在状态，当然其也是大厂笔试的必考题。

所以很有必要总结一下，对今后的学习也是很有帮助的。

HTTP状态码总的分为五类：

1开头：信息状态码

2开头：成功状态码

3开头：重定向状态码

4开头：客户端错误状态码

5开头：服务端错误状态码

 1XX：信息状态码

状态码	含义	描述
100	继续	初始的请求已经接受，请客户端继续发送剩余部分
101	切换协议	请求这要求服务器切换协议，服务器已确定切换
 2XX：成功状态码

状态码	含义	描述
200	成功	服务器已成功处理了请求
201	已创建	请求成功并且服务器创建了新的资源
202	已接受	服务器已接受请求，但尚未处理
203	非授权信息	服务器已成功处理请求，但返回的信息可能来自另一个来源
204	无内容	服务器成功处理了请求，但没有返回任何内容
205	重置内容	服务器处理成功，用户终端应重置文档视图
206	部分内容	服务器成功处理了部分GET请求
3XX：重定向状态码

状态码	含义	描述
300	多种选择	针对请求，服务器可执行多种操作
301	永久移动	请求的页面已永久跳转到新的url
302	临时移动	服务器目前从不同位置的网页响应请求，但请求仍继续使用原有位置来进行以后的请求
303	查看其他位置	请求者应当对不同的位置使用单独的GET请求来检索响应时，服务器返回此代码
304	未修改	自从上次请求后，请求的网页未修改过
305	使用代理	请求者只能使用代理访问请求的网页
307	临时重定向	服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求
4XX：客户端错误状态码

状态码	含义	描述
400	错误请求	服务器不理解请求的语法
401	未授权	请求要求用户的身份演验证
403	禁止	服务器拒绝请求
404	未找到	服务器找不到请求的页面
405	方法禁用	禁用请求中指定的方法
406	不接受	无法使用请求的内容特性响应请求的页面
407	需要代理授权	请求需要代理的身份认证
408	请求超时	服务器等候请求时发生超时
409	冲突	服务器在完成请求时发生冲突
410	已删除	客户端请求的资源已经不存在
411	需要有效长度	服务器不接受不含有效长度表头字段的请求
412	未满足前提条件	服务器未满足请求者在请求中设置的其中一个前提条件
413	请求实体过大	由于请求实体过大，服务器无法处理，因此拒绝请求
414	请求url过长	请求的url过长，服务器无法处理
415	不支持格式	服务器无法处理请求中附带媒体格式
416	范围无效	客户端请求的范围无效
417	未满足期望	服务器无法满足请求表头字段要求

5XX：服务端错误状态码
状态码	含义	描述
500	服务器错误	服务器内部错误，无法完成请求
501	尚未实施	服务器不具备完成请求的功能
502	错误网关	服务器作为网关或代理出现错误
503	服务不可用	服务器目前无法使用
504	网关超时	网关或代理服务器，未及时获取请求
505	不支持版本	服务器不支持请求中使用的HTTP协议版本
```
### 七层网络模型
```
应用层
应用层是网络体系中最高的一层，也是唯一面向用户的一层，也可视为为用户提供常用的应用程序，每个网络应用都对应着不同的协议
HTTP、TFTP, FTP, NFS, WAIS、SMTP

表示层
主要负责数据格式的转换，确保一个系统的应用层发送的消息可以被另一个系统的应用层读取，编码转换，数据解析，管理数据的解密和加密，同时也对应用层的协议进行翻译

Telnet, Rlogin, SNMP, Gopher

会话层
负责网络中两节点的建立，在数据传输中维护计算机网络中两台计算机之间的通信连接，并决定何时终止通信
SMTP, DNS

传输层
是整个网络关键的部分，是实现两个用户进程间端到端的可靠通信，处理数据包的错误等传输问题。是向下通信服务最高层，向上用户功能最底层。即向网络层提供服务，向会话层提供独立于网络层的传送服务和可靠的透明数据传输。
TCP, UDP

网络层
进行逻辑地址寻址，实现不同网络之间的路径选择，IP就在网络层
IP, ICMP, ARP, RARP, AKP, UUCP

数据链路层:
物理地址（MAC地址），网络设备的唯一身份标识。建立逻辑连接、进行硬件地址寻址，相邻的两个设备间的互相通信
FDDI, Ethernet, Arpanet, PDN, SLIP, PPP，STP。HDLC,SDLC,帧中继
物理层:
七层模型中的最底层，主要是物理介质传输媒介（网线或者是无线），在不同设备中传输比特，将0/1信号与电信号或者光信号互相转化
IEEE 802.1A, IEEE 802.2到IEEE 802
```