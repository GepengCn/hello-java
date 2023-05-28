# Netty

## 问题

- BIO, NIO, AIO 有啥区别？
- Netty是什么？
- 为啥不直接用NIO呢？
- 为什么要用Netty？
- Netty应用场景了解吗？
- 哪些开源项目用到了Netty？
- 介绍下Netty的核心组件？
- NioEventLoopGroup默认构造函数会起多少线程？
- Reactor线程模型？
- Netty线程模型？
- Netty服务端和客户端的启动过程了解吗？
- 什么是TCP粘包/拆包？有什么解决办法呢？
- TCP长连接、短连接了解吗？
- Netty长连接、心跳机制了解吗？
- Netty的零拷贝了解吗？

## 答案

### BIO, NIO, AIO 有啥区别？

!!! tip "BIO, NIO, AIO 有啥区别？"

    - BIO:同步阻塞，读写阻塞在一个线程内等待完成
    - NIO:同步非阻塞：基于缓冲、通道的，与BIO相对的Socket是SocketChannel， ServerSocket是ServerSocketChannel，支持非阻塞模式
    - AIO:异步非阻塞：基于事件和回调，操作后立刻返回，不会阻塞，当io就绪会交给回调来处理，而NIO是io就绪自己处理。
    
### Netty是什么？

!!! tip "Netty是什么？"

    - 基于NIO的client-server框架，可以快速开发网络应用
    - 极大简化TCP和UDP网络编程，性能和安全性也更好
    - 支持多种协议，http、ftp、smtp、文本、二进制等

### 为啥不直接用NIO呢？

!!! tip "为啥不直接用NIO呢？"
    
    - NIO编程模型复杂，而且存在一些bug
    - 对断线重连、丢包、粘包等处理起来很复杂

### 为什么要用Netty？

!!! tip "为什么要用Netty？"

    - 性能更好、延迟更低
    - 成熟稳定
    - 自带编解码器解决TCP粘包/拆包
    - 支持多种协议
    - API良好，使用简单

### Netty应用场景了解吗？

!!! tip "Netty应用场景了解吗？"

    - 用来研发RPC框架
    - 实现http服务器
    - 即使通讯
    - 消息推送

### 哪些开源项目用到了Netty？

!!! tip "哪些开源项目用到了Netty？"

    Dubbo RocketMQ ES gRPC


### 介绍下Netty的核心组件？

!!! tip "介绍下Netty的核心组件？"

    - ByteBuff：字节容器,相当于NIO中ByteBuffer的封装，使用更简单
    - Bootstrap和ServerBootstrap：启动引导类，指定连接的主机和端口，配置线程组
    - Channel：读写数据等操作，服务端是NioServerSocketChannel，客户端是NioSocketChannel
    - EventLoop：负责注册到其上的Channel的读写操作，两者配合，一个EventLoop对应多个Channel
    - ChannelHandler：消息具体处理，收发消息
    - ChannelPipeline：链式注册ChannelHandler
    - ChannelFuture：获取操作执行结果


### NioEventLoopGroup默认构造函数会起多少线程？

!!! tip "NioEventLoopGroup默认构造函数会起多少线程？"

    CPU核心数*2

### Reactor线程模型？

!!! tip "Reactor线程模型？"

    - 单线程：所有操作都由一个线程处理
    - 多线程：一个线程负责接受请求，一组线程处理io
    - 主从：一组线程负责接受请求，一组线程处理io

### Netty线程模型？

!!! tip "Netty线程模型？"

    - 单线程：引导类注册接受和处理io都是EventLoopGroup(1)
    - 多线程：接受是EventLoopGroup(1)，处理io是EventLoopGroup()
    - 主从：接受和处理io都是EventLoopGroup()

### Netty服务端和客户端的启动过程了解吗？

!!! tip "Netty服务端和客户端的启动过程了解吗？"

    1. 配置接受请求和处理io的EventLoopGroup
    2. 通过引导类注册事件循环组，指定io模型（一般是NioServerSocketChannel.class），注册处理handler（包含自定义的处理类）,绑定端口然后sync方法同步阻塞
    3. 最后再配置个优雅停机

### 什么是TCP粘包/拆包？有什么解决办法呢？

!!! tip "什么是TCP粘包/拆包？有什么解决办法呢？"

    客户端发送一大串，服务端收到消息揉在一起或者被拆分了
    
    使用Netty自带的一些解码器
    - LineBasedFrameDecoder：按换行符分隔
    - FixedLengthFrameDecoder：固定长度拆包，不够的空格补全
    - HttpRequestDecoder:http协议的解码器


### TCP长连接、短连接了解吗？

!!! tip "TCP长连接、短连接了解吗？"

    TCP读写前后，需要经历三次握手和四次挥手，短连接指的建立连接后，读写完成后就关闭连接，下次再需要读写，还需要重写连接。
    
    长连接，指的是建立连接后，读写一次也不会关闭连接，后续读写可以复用这个连接。
    
    https://mp.weixin.qq.com/s?__biz=MzA3NzI1Njk1MQ==&mid=2648577524&idx=1&sn=b18b43a632e9a75f860960d886e2cc87&chksm=877e746cb009fd7a2cf597138e907abecbe062e9cb4fe8ada7fda61bca3debc2ace871d2615e&scene=27

### Netty长连接、心跳机制了解吗？

!!! tip "Netty长连接、心跳机制了解吗？"

    长连接过程中，可能会出现断网、异常等等发生，如果期间client和server没有交互的话，无法发现对方已掉线。
    
    client和server一段时间没交互，即idle状态，客户端或服务端会给对方发一个数据包，对方收到回返一个数据包，也就是一个ping-pong交互。
    
    TCP本身自带心跳机制，`SO_KEEPALIVE`。但是长连接的不如netty自定义的灵活，核心类是IdleStateHandler



### Netty的零拷贝了解吗？

!!! tip "Netty的零拷贝了解吗？"
    
    - 使用堆外内存，避免了堆内外的内存拷贝
    - 使用CompositeByteBuf类，可以将多个ByteBuf合成一个，避免了各个ByteBuf之间的拷贝
    - 包装的ByteBuf对象，共用一个内存空间，不会产生内存拷贝
    - ByteBuf的slice，可以将一个拆分成多个
    - FileChannel的tranferTo实现的文件传输，直接将文件缓冲区的数据发送到目标Channel，避免了传统write方式导致的内存拷贝