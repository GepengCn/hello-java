# 计算机网络基础

## 问题

- OSI 七层模型是什么？每一层的作用是什么？
- TCP/IP 四层模型是什么？每一层的作用是什么？
- 为什么网络要分层？
- 应用层有哪些常见的协议？
- 传输层有哪些常见的协议？
- 网络层有哪些常见的协议？
- 从输入 URL 到页面展示到底发生了什么？（非常重要）
- HTTP 状态码有哪些？
- HTTP Header 中常见的字段有哪些？
- HTTP 和 HTTPS 有什么区别？（重要）
- HTTP/1.0 和 HTTP/1.1 有什么区别？
- HTTP/1.1 和 HTTP/2.0 有什么区别？
- HTTP/2.0 和 HTTP/3.0 有什么区别？
- HTTP 是不保存状态的协议, 如何保存用户状态?
- URI 和 URL 的区别是什么?
- Cookie 和 Session 有什么区别？
- PING 命令的作用是什么？
- PING 命令的工作原理是什么？
- DNS 的作用是什么？
- DNS 服务器有哪些？
- DNS 解析的过程是什么样的？
- TCP三次握手
- TCP四次挥手
- TCP 与 UDP 的区别（重要）
- 什么时候选择 TCP，什么时候选 UDP?
- HTTP 基于 TCP 还是 UDP？
- 使用 TCP 的协议有哪些?使用 UDP 的协议有哪些?
- TCP 三次握手和四次挥手（非常重要）
- TCP 如何保证传输的可靠性？（重要）
- TCP 如何实现流量控制？
- TCP 为什么需要流量控制？
- TCP 的拥塞控制是怎么实现的？



## 答案

### OSI 七层模型是什么？每一层的作用是什么？

!!! tip "OSI 七层模型是什么？每一层的作用是什么？"

    ![](https://p.ipic.vip/j5k2r0.jpg)

    - 应用层：各种协议，比如http ftp
    - 表示层：编解码，加解密、压缩解压缩
    - 会话层：会话建立、维护、重连
    - 传输层：主机之间数据传输 TCP UDP
    - 网络层：路由和寻址 IP
    - 数据链路层：帧编码和误差纠正，物理地址
    - 物理层：比特流传输

### TCP/IP 四层模型是什么？每一层的作用是什么？

!!! tip "TCP/IP 四层模型是什么？每一层的作用是什么？"

    - 应用层

    定义了信息交换的格式

    - 传输层

    向两台终端提供数据传输服务

    - 网络层

    寻址、路由、流量控制

    - 网络接口层

    把网络层封装好的数据，传输到目标计算机上，该层是对数据链路层和物理层的合并


### 为什么网络要分层？

!!! tip "为什么网络要分层？"

    类似于我们开发后台框架要分层controller层，service层一样，网络分层能让每层专注在一件事上面，互相之间相互独立，更灵活

### 应用层有哪些常见的协议？

!!! tip "应用层有哪些常见的协议？"

    http, smtp, ftp,ssh

### 传输层有哪些常见的协议？

!!! tip "传输层有哪些常见的协议？"

    tcp udp

### 网络层有哪些常见的协议？
!!! tip "网络层有哪些常见的协议？"

    ip, nat

### 从输入 URL 到页面展示到底发生了什么？（非常重要）

!!! tip "从输入 URL 到页面展示到底发生了什么？（非常重要）"

    1. DNS 解析，获取域名对应ip
    2. TCP 连接
    3. 发送 HTTP 请求
    4. 服务器处理请求并返回 HTTP 报文
    5. 浏览器解析渲染页面
    6. 连接结束

### HTTP 状态码有哪些？

!!! tip "HTTP 状态码有哪些？"

    200:请求成功
    201:代表创建成功
    202:接收未处理
    204:收到未返回内容


    301 被永久重定向了
    302 资源被临时重定向

    400 请求存在问题 比如请求参数不合法、请求方法错误
    401 未认证
    403 非法请求
    404 未找到资源
    409 存在冲突

    500 服务端出问题
    502 网关通了，但是服务器返回错误


### HTTP Header 中常见的字段有哪些？

!!! tip "HTTP Header 中常见的字段有哪些？"

    Accept 内容类型
    Accept-Charset 字符集
    Connection 连接类型
    Content-Length 内容长度
    Content-Type 请求体的内容类型
    Cookie
    Host 服务器域名

### HTTP 和 HTTPS 有什么区别？（重要）

!!! tip "HTTP 和 HTTPS 有什么区别？（重要）"

    http 运行在tcp上，内容明文
    https 运行在ssl/tls 内容密文

    搜索引擎更青睐https

    端口和前缀不一样

### HTTP/1.0 和 HTTP/1.1 有什么区别？

!!! tip "HTTP/1.1 和 HTTP/2.0 有什么区别？"

    1.0短连接

    1.1 还支持长连接

    1.1增加了很多状态码

    1.1 增加了host字段，同一个ip托管多个域名

### HTTP/1.1 和 HTTP/2.0 有什么区别？

!!! tip "HTTP/1.1 和 HTTP/2.0 有什么区别？"

    IO 多路复用:减少延迟，性能更好

    二进制帧:报文更小，减少传输数据量

    头部压缩:减少网络开销

    服务器推送:请求时，服务器将其他资源一并推给客户端，减少请求次数和延迟

### HTTP/2.0 和 HTTP/3.0 有什么区别？

!!! tip "HTTP/2.0 和 HTTP/3.0 有什么区别？"

    传输协议: 2.0 TCP  3.0 QUIC

    连接建立:3.0不需要3此握手

    队头阻塞:3.0多路复用+轮询

    3.0 具有更好的错误恢复机制,安全性更高



###  HTTP 是不保存状态的协议, 如何保存用户状态?
!!! tip " HTTP 是不保存状态的协议, 如何保存用户状态?"

    使用session保存，再使用cookie附加sessionId追踪

### URI 和 URL 的区别是什么?


!!! tip "URI 和 URL 的区别是什么?"

    uri 标识资源

    url 标识资源+定位

### Cookie 和 Session 有什么区别？

!!! tip "Cookie 和 Session 有什么区别？"


    session 服务端记录用户的状态

    cookie 保存在客户端


### PING 命令的作用是什么？

!!! tip "PING 命令的作用是什么？"

    测试主机之间的连通性和网络延迟。

### PING 命令的工作原理是什么？

!!! tip "PING 命令的工作原理是什么？"

    通过在网络上发送和接收 ICMP 报文


### DNS 的作用是什么？

!!! tip "DNS 的作用是什么？"

    解决的是域名和 IP 地址的映射问题

### DNS 服务器有哪些？

!!! tip "DNS 服务器有哪些？"

    根 DNS 服务器

    顶级域 DNS 服务器 com cn org net edu

    权威 DNS 服务器

    本地 DNS 服务器


### DNS 解析的过程是什么样的？

!!! tip "DNS 解析的过程是什么样的？"

    - 本地缓存
    - 本地hosts
    - 本地dns服务器
    - 根dns服务器
    - 顶级dns服务器地址
    - 发送解析请求
    - 返回域名服务器地址
    - 查询域名和ip映射关系
    - 本地dns缓存这个映射关系
    - 解析结果返回本地电脑

### TCP三次握手

!!! tip "TCP三次握手"

    - 客户端发送SYN到服务端，客户端进入SYN_SEND状态，等待服务端确认
    - 服务端发送SYN+ACK到客户端，服务器进入SYN_RECV状态
    - 客户端发送ACK到服务端，客和服都处于ESTABLISHED状态，完成握手

### TCP四次挥手

!!! tip "TCP四次挥手"

    - 客户端发送FIN到服务端，客户端进入FIN-WAIT-1状态
    - 服务端收到后，返回一个ACK给客户端，服务端进入CLOSE-WAIT状态，客户端进入FIN-WAIT-2状态
    - 服务端关闭与客户端连接，并发送一个FIN给客户端，服务端进入LAST-ACK状态
    - 客户端发送ACK给服务端，并且进入TIME-WAIT状态，服务端收到后进入CLOSE状态。如果客户端2MSL没收到回复，证明服务端正常关闭，客户端可以关闭了。

### TCP 与 UDP 的区别（重要）

!!! tip "TCP 与 UDP 的区别（重要）"

    1. tcp要先建立连接，而udp不用
    2. tcp保证可靠传输，而udp不保证
    3. tcp有保存是否发送是否接收等状态，udp没有
    4. tcp需要连接确认重传等，效率比udp要低
    5. tcp面向字节流，udp面向报文
    6. 头部开销，tcp要比udp大
    7. tcp只支持点对点，udp还可以支持1对多，多对多


### 什么时候选择 TCP，什么时候选 UDP?

!!! tip "什么时候选择 TCP，什么时候选 UDP?"


    UDP 一般用于即时通信,比如：语音、 视频、直播

    TCP 用于对传输准确性要求特别高的场景，比如文件传输、发送和接收邮件、远程登录等等。

### HTTP 基于 TCP 还是 UDP？

!!! tip "HTTP 基于 TCP 还是 UDP？"

    http3.0之前是基于tcp，3.0是基于udp

### 使用 TCP 的协议有哪些?使用 UDP 的协议有哪些?

!!! tip "使用 TCP 的协议有哪些?使用 UDP 的协议有哪些?"

    tcp

    http https ftp smtp pop3/imap ssh

    udp

    DHCP

    dns


### TCP 三次握手和四次挥手（非常重要）

!!! tip "TCP 三次握手和四次挥手（非常重要）"

    - [x] 为什么要三次握手?

    双方确认自己与对方的发送与接收是正常的

    - [x] 第 2 次握手传回了 ACK，为什么还要传回 SYN？

    为了告诉客户端：“我接收到的信息确实就是你所发送的信号了”

    - [x] 为什么要四次挥手？

    因为一方要关的时候另一方可能还没传输完，所以需要双方都挥手+响应挥手之后，才能真正的关闭连接，才不会丢失数据

    - [x] 为什么不能把服务器发送的 ACK 和 FIN 合并起来，变成三次挥手？

    因为服务端可能还没传输完，所以先回复ACK表示收到，等待传输彻底之后，再回复FIN断开请求

    - [x] 如果第二次挥手时服务器的 ACK 没有送达客户端，会怎样？

    客户端没收到ACK确认，会重新发

    - [x] 为什么第四次挥手客户端需要等待 2*MSL（报文段最长寿命）时间后才进入 CLOSED 状态？

    如果没有这个等待时间，客户端直接关闭了并且第四次挥手的报文丢失了，服务端就会一直处于确认状态无法关闭，有了这个时间，服务器才可以重试发送并且正常关闭


### TCP 如何保证传输的可靠性？（重要）

!!! tip "TCP 如何保证传输的可靠性？（重要）"

    1. 分块传输
    2. 每个包都有序号，保证不会重复和丢包
    3. 数据校验
    4. 超时重传
    5. 流量控制
    6. 拥塞控制

### TCP 如何实现流量控制？

!!! tip "TCP 如何实现流量控制？"

    滑动窗口。确认报文中的窗口字段设置窗口大小，如果设置为0，发送方就不能发送数据


### 为什么需要流量控制?

!!! tip "为什么需要流量控制? "

    收发双方速率不等，发的过快，接收的处理不过来，就会放入缓冲区。但是缓冲区放不下就会丢掉，这样又丢包又浪费网络资源


### TCP 的拥塞控制是怎么实现的？

!!! tip "TCP 的拥塞控制是怎么实现的？"

    拥塞控制：防止过多的数据注入到网络，使网络中的路由器或链路不致过载

    - 全局的

    - 发送方维持一个拥塞窗口(cwnd) 变量，大小取决于网络拥塞程度，会从发送和接收方取更小值

    - 慢开始

    - 缓慢增大

    - 快重传与快恢复，接收方收到收到三个重复确认，会假定数据段丢失，并立即重传


