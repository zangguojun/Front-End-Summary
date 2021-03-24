# HTTP版本迭代

![img](https://img-blog.csdnimg.cn/20201026004805676.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMzY5OTc5,size_16,color_FFFFFF,t_70)

## 1、HTTP0.9

仅仅能支持get方法，并且只能请求和响应HTML格式的资源



## 2、HTTP1.0

开始包含头信息，并且增加POST和HEAD请求方法

![img](https://img-blog.csdnimg.cn/20201026004933436.png)

同时增加了响应体的内容格式：

- text/html
- text/json
- image/png
- image/jpeg
- ....

一个典型的HTTP响应报文的响应头：

```
Content-Language: zh-CN
Date: Sun, 25 Oct 2020 16:31:09 GMT
Content-Length: 43756
Content-Type: text/html;charset=UTF-8
Server: Apache-Coyote/1.1
Set-Cookie: JSESSIONID=dWi7X6oNWfBN-88xGmDuFq94.undefined; Path=/
```

可以看到响应报文头主要以Key:Value方式记录了元信息：

响应体长度、响应体格式、字符编码、服务器名称、以及服务器返回的SessionID等

> SessionID为服务器产生的一个客户端会话标识，客户端下一次HTTP请求报文中带上该SessionID，那么服务器将确认客户端身份，而无需进行认证，SessionID在客户端通常存放于本地的Cookie中

## 2、HTTP1.1

**①：引入长连接机制**

​	在完成一个HTTP请求=》响应后，TCP连接不关闭，而是可以继续被复用

![img](https://img-blog.csdnimg.cn/20201026011006544.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMzY5OTc5,size_16,color_FFFFFF,t_70)

在建立起TCP连接后，这个连接可不断的传输多个HTTP请求报文和响应报文，其基于TCP长连接机制，默认timeout为：7200s

因此其相对于HTTP1.0主要**改善点**：

1. 避免频繁的建立和关闭TCP连接：三次握手和四次挥手带来的开销和时延。
2. 避免TCP刚建立连接时，拥塞控制算法造成的低传输效率，主要是慢启动阶段

**②：管道机制实现同时发送多个HTTP请求**

另外HTTP1.1还基于管道机制，使得一个TCP连接中，允许发送多个请求

1. HTTP1.0中：一个请求发出后，客户端需要等待服务器响应完成后，才能发送下一个HTTP请求
2. HTTP1.1中：客户端可同时发出多个HTTP请求，避免长时间等待在耗时的HTTP请求中。

![img](https://img-blog.csdnimg.cn/20201026012550690.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMzY5OTc5,size_16,color_FFFFFF,t_70)

**③：Content-length 与分块传输**

Conetnt-length字段位于HTTP响应报文中，用于确定响应体的字节长度。

客户端从TCP缓冲区中：

先读取响应行、响应体、以及一个空行，解析出HTTP报文的头部信息和元数据（文本格式）。根据**Content-length字段**，从缓冲区读取指定的长度字节。从而**分割不同的HTTP响应报文**。

![img](https://img-blog.csdnimg.cn/20201026013537541.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMzY5OTc5,size_16,color_FFFFFF,t_70)

若响应体的内容非常大，或者是不断增大的，在服务器封装HTTP响应报文头时**无法确切知道Content-length大小**。HTTP1.1解决方法是，采取流模式分块传输：即不使用Content-length字段，而是Transfer-Encoding：Chunked 在响应体中，每一个数据分块前面指明大小16进制数，最后一个数字0表示结束

④：文件断点续传

文件通过HTTP协议传输时，其本质基于TCP实现可靠的传输

通常基于HTTP下载文件中，已传输的部分通过文件系统写入到临时文件（.temp .download等格式中）

当发生暂停或者网络断开，下一次继续重传时，HTTP请求报文中，添加**已经传输部分的长度**。

## 2、HTTP2.0

**双工模式**：不仅仅客户端可以发生多个HTTP请求，服务端也可以同时响应多个HTTP请求。

基于**IO复用**技术，一个线程可以处理多个HTTP请求，避免队头堵塞的现象。同时HTTP多个请求可能包含大量的HTTP请求头字段重复。HTTP2.0采取字段表形式，传输不再传输完整字段，可以仅仅传输相应的字段索引。

二进制协议：HTTP1.1其请求行、请求头为文本格式，而请求体可以不同的格式
而HTTP2.0头信息和体信息均为二进制格式，即所谓的头信息帧和数据帧。

HTTP/2引入了“服务端推（server push）”的概念，它允许服务端在客户端需要数据之前就主动地将数据 发送到客户端缓存中，从而提高性能。 HTTP/2提供更多的加密支持 HTTP/2使用多路技术，允许多个消息在一个连接上同时交差。 它增加了头压缩（header compression），因此即使非常小的请求，其请求和响应的header都只会占 用很小比例的带宽。

### 1.二进制传输

**HTTP/2传输数据量的大幅减少,主要有两个原因:以二进制方式传输和Header 压缩**。我们先来介绍二进制传输,HTTP/2 采用二进制格式传输数据，而非HTTP/1.x 里纯文本形式的报文 ，二进制协议解析起来更高效。**HTTP/2 将请求和响应数据分割为更小的帧，并且它们采用二进制编码**。

它把TCP协议的部分特性挪到了应用层，把原来的"Header+Body"的消息"打散"为数个小片的二进制"帧"(Frame),用"HEADERS"帧存放头数据、"DATA"帧存放实体数据。HTP/2数据分帧后"Header+Body"的报文结构就完全消失了，协议看到的只是一个个的"碎片"。



### 2.Header 压缩

HTTP/2并没有使用传统的压缩算法，而是开发了专门的"HPACK”算法，在客户端和服务器两端建立“字典”，用索引号表示重复的字符串，还采用哈夫曼编码来压缩整数和字符串，可以达到50%~90%的高压缩率。

具体来说:

- 在客户端和服务器端使用“首部表”来跟踪和存储之前发送的键-值对，对于相同的数据，不再通过每次请求和响应发送；
- 首部表在HTTP/2的连接存续期内始终存在，由客户端和服务器共同渐进地更新;
- 每个新的首部键-值对要么被追加到当前表的末尾，要么替换表中之前的值

例如下图中的两个请求， 请求一发送了所有的头部字段，第二个请求则只需要发送差异数据，这样可以减少冗余数据，降低开销

<img src="https://ss.csdn.net/p?https://mmbiz.qpic.cn/mmbiz_png/zewrLkrYfsPJcceCMInZpsVErpdiaXm87W3C6GXr1CDt3avVmWsURbboBrKJDkPaUbMlKDfQOx1jllg810ibdYiaQ/640?wx_fmt=png" alt="640?wx_fmt=png" style="zoom:150%;" />



### 3.多路复用

在 HTTP/2 中引入了多路复用的技术。多路复用很好的解决了浏览器限制同一个域名下的请求数量的问题，同时也接更容易实现全速传输，毕竟新开一个 TCP 连接都需要慢慢提升传输速度。

大家可以通过 该链接 直观感受下 HTTP/2 比 HTTP/1 到底快了多少。

