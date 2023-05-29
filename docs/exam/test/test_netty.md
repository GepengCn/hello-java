- BIO, NIO, AIO 有啥区别？

bio 同步阻塞，一个线程接受并处理io读写
nio 同步非阻塞，基于通道和缓冲区的，数据在通道和缓冲区内流转实现的读写，通过selector选择器，可以一个线程监听多个连接。
aio 异步非阻塞，基于事件和回调，操作过来立刻返回，处理好io，走回调

- Netty是什么？

一个高性能的网络io框架

- 为啥不直接用NIO呢？

实现起来很复杂，还有一些bug

- 为什么要用Netty？

- 性能更好，稳定成熟，很多开源框架都在用
- 使用简单，而且功能丰富强大
- 支持各种协议

- Netty应用场景了解吗？

- web服务
- rpc
- 即时通讯
- 消息推送

- 哪些开源项目用到了Netty？

Dubbo，gRPC

- 介绍下Netty的核心组件？

EventLoopGroup

Bootstrap

ChannelPipeline

Channel



- NioEventLoopGroup默认构造函数会起多少线程？

cpu*2

- Reactor线程模型？

单线程
一个线程处理请求和io
多线程
一个线程处理请求，多个线程处理io
主从
多个线程处理请求，多个线程处理io

- Netty线程模型？

group(1)

boss(1) work()

boss() work()

- Netty服务端和客户端的启动过程了解吗？

先配置线程模型，接收请求和处理io分别配置EventLoopGroup

引导类注册模型，指定nio，绑定端口或者主机，sync方法启动

返回ChannelFuture监控是否启动成功

增加一个优雅停机

Runtime.shutdownHook{
    worker/boss.shutdownGraceully()    
}

- 什么是TCP粘包/拆包？有什么解决办法呢？

客户端发送一堆，接收的时候揉在一块，或者发送一个大包，被拆分了

使用Netty提供的各种拆包解析器

按一行拆，按固定大小拆，或者按协议拆，常用的httprequestdecoder

- TCP长连接、短连接了解吗？

tcp简历链接要经历三次握手，四次挥手

短连接就是处理完业务，立刻断开，在处理，需要重新连接

长连接是连接成功，处理完也不断开，再有处理，可以复用

- Netty长连接、心跳机制了解吗？

建立长连接之后，在一段时间没有消息收发时，一方会给另一方发送个消息包ping，接收方收到后返回个数据包pong，表明双方都在线，避免一方掉线了，另一方不知道

IdleStateHandler

- Netty的零拷贝了解吗？

使用对外内存，避免了数据在堆内外的来回拷贝

ByteBuf 封装nio的ByteBuffer，是逻辑调用，用的还是原来的内存地址，避免了拷贝

Netty可以把多个ByteBuffer合并成一个大的，其实内存地址也都没变，也是逻辑合并了

拆分同理

FileRegion 在传输文件时，直接把文件流从缓冲区推送到通道，没有使用传统的write，避免了拷贝