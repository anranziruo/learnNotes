## 内容

### 简介
```
HTTPS是一种应用层协议，本质上来说它是HTTP协议的一种变种。HTTPS比HTTP协议安全，因为HTTP是明文传输，而HTTPS是加密传输，加密过程中使用了三种加密手段，分别是证书，对称加密和非对称加密。HTTPS相比于HTTP多了一层SSL/TSL，其构造如下：
```
### https进行的流程
```
服务器端的公钥和私钥，用来进行非对称加密
客户端生成的随机密钥，用来进行对称加密
1.客户端向服务器发起HTTPS请求，连接到服务器的443端口
2.服务器端有一个密钥对，即公钥和私钥，是用来进行非对称加密使用的，服务器端保存着私钥，不能将其泄露，公钥可以发送给任何人。
3.服务器将自己的公钥发送给客户端。
4.客户端收到服务器端的公钥之后，会对公钥进行检查，验证其合法性，如果发现发现公钥有问题，那么HTTPS传输就无法继续。严格的说，这里应该是验证服务器发送的数字证书的合法性，关于客户端如何验证数字证书的合法性，下文会进行说明。如果公钥合格，那么客户端会生成一个随机值，这个随机值就是用于进行对称加密的密钥，我们将该密钥称之为client key，即客户端密钥，这样在概念上和服务器端的密钥容易进行区分。然后用服务器的公钥对客户端密钥进行非对称加密，这样客户端密钥就变成密文了，至此，HTTPS中的第一次HTTP请求结束。
5.客户端会发起HTTPS中的第二个HTTP请求，将加密之后的客户端密钥发送给服务器。
6.服务器接收到客户端发来的密文之后，会用自己的私钥对其进行非对称解密，解密之后的明文就是客户端密钥，然后用客户端密钥对数据进行对称加密，这样数据就变成了密文。
7.然后服务器将加密后的密文发送给客户端。
8.客户端收到服务器发送来的密文，用客户端密钥对其进行对称解密，得到服务器发送的数据。这样HTTPS中的第二个HTTP请求结束，整个HTTPS传输完成。
```
### HTTPS和HTTP的区别
```
1:HTTPS是密文传输，HTTP是明文传输；
2:默认连接的端口号是不同的，HTTPS是443端口，而HTTP是80端口；
3:HTTPS请求的过程需要CA证书要验证身份以保证客户端请求到服务器端之后，传回的响应是来自于服务器端，而HTTP则不需要CA证书；
4:HTTPS=HTTP+加密+认证+完整性保护。
```