- BIO, NIO, AIO 有啥区别？
bio:同步阻塞，一个线程内处理请求以及io读写
nio：同步非阻塞，基于缓存区和通道的
aio：异步非阻塞，基于事件和回调的，处理请求和io操作是直接返回的，等到io就绪，在回调方法里执行就绪后的操作，nio是同步等待io就绪才执行后续
- Netty是什么？

一个高可用的io网络框架


- 为啥不直接用NIO呢？

- 用nio开发非常繁琐，而且还有一些bug
- 一些高级的特性，比如粘包拆包，或者是各种协议的支持等开发起来非常复杂，没有Netty那么简单强大

- 为什么要用Netty？

- api简单强大
- 提供各种拆包粘包的方式，对各种协议的支持都非常的全
- 性能非常好
- 成熟稳定，很多大厂，成熟产品都在用
- 文档良好
- 社区活跃

- Netty应用场景了解吗？

web服务、消息推送、即时通讯

- 哪些开源项目用到了Netty？
Dubbo es rocketmq grpc
- 介绍下Netty的核心组件？

EventLoopGroup，配置Netty的线程模型
Bootstrap，引导类+配置
ChannelHandler：粘包拆包、协议的支持、自定义的逻辑
ChannelPipeline：将ChannelHandler链式调用起来
Channel：通道
EventLoop：执行线程
ChannelFuture：返回的结果

- NioEventLoopGroup默认构造函数会起多少线程？
cpu*2
- Reactor线程模型？

单线程：1一个线程处理请求和io
多线程：1个线程处理请求，多个线程处理io
主从：多个线程处理请求，多个线程处理io

- Netty线程模型？

同上

- Netty服务端和客户端的启动过程了解吗？

先配置线程模型，然后引导，指定nio，pipeline链配置，粘包拆包、协议处理、自定义的任务、指定端口或者主机，sync方法启动，配置优雅停机

- 什么是TCP粘包/拆包？有什么解决办法呢？

客户端一次性发送多个包揉在一起，或者一个大包被拆成多个。

使用Netty提供的编解码器

LineBased
Fixed
Http

- TCP长连接、短连接了解吗？

短连接，就是一个请求过来建立tcp连接，三次握手，然后请求后，断开连接，四次挥手
长连接，就是一个请求建立连接之后，不断开，有新请求可以复用这个链接

- Netty长连接、心跳机制了解吗？

服务端和客户端长时间没有交互，为了避免对方停机了不知道，会发送一个ping给对方，对方收到后返回pong的数据包。

- Netty的零拷贝了解吗？

- nio，使用堆外内存，直接分配内存
- ByteBuf可以合成或者拆分，其实都是逻辑的，内存地址还是原来的地址
- 文件复制时，文件直接写入通道缓冲区，不用原始的write形式