### 什么是HTTP？
#### 超文本传输协议（Hypertext Transfer Protocol），用于在Web浏览器和网站服务器之间传输超文本。
#### HTTP三次握手
#### 非持久连接，如果要传输多次数据，需要多次建立连接，建立连接的过程需要握手，断开连接需要四次挥手，非常浪费时间和资源，
所以出现了HTTP1.1的keep-alive，使得在一个http连接中，可以发送多个Request，接收多个Response，并且，一个Request对应一个Response，
服务器也不能主动push数据给客户端。

### 什么是HTTPS？
#### 超文本传输安全协议（Hypertext Transfer Protocol Secure），利用SSL/TLS（传输层协议）来加密数据包，也即在传输层对HTTP传输的内容进行加密后
通过TCP/IP网络。
#### HTTPS的握手过程


#### 什么是WebSocket？
#### 
