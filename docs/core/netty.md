# Netty

![](https://p.ipic.vip/qrrk7m.jpg)

### 什么不直接用NIO

!!! tip "什么不直接用NIO"

    NIO编程模型复杂，而且存在一些bug

### 为什么用Netty?

!!! tip "为什么用Netty?"

    - 性能更好、延迟更低
    - 成熟稳定
    - 自带编解码器解决TCP粘包/拆包
    - 支持多种协议
    - API良好，使用简单

### 什么是Channel

!!! tip "什么是Channel"

    读写I/O操作，线程安全的，isActive， writeAndFlush
    
### Channel生命周期

!!! tip "Channel生命周期"

    - ChannelRegistered: 已经注册到EventLoop
    - ChannelActive: 处于活动状态，可以收发数据了
    - ChannelInactive: 没有连接到远程节点
    - ChannelUnregistered: Channel已经被创建，但还未注册到EventLoop
    
    顺序为ChannelRegistered->ChannelActive->ChannelInactive->ChannelUnregistered


### Channel-NIO

!!! tip "Channel-NIO"

    I/O操作的全异步实现，Channel的状态变化时（读、写、连接、注册就绪）收到通知。支持零拷贝。
    
    不是完全非阻塞的，比如FileChannel

### Channel-Epoll

!!! tip "Channel-Epoll"

    Linux独有，基于linux的epoll特性，完全非阻塞。性能要高于NIO。

### Channel-OIO

!!! tip "Channel-OIO"

    支持阻塞的代码库

### Channel-Local

!!! tip "Channel-Local"

    同一个jvm的异步通信

### Channel-Embedded

!!! tip "Channel-Embedded"

    测试ChannelHandler的实现


### ByteBuf有什么优点

!!! tip "ByteBuf有什么优点"

    - 可自定义扩展
    - 合并和拆分实现了零拷贝
    - 读写模式切换不需要调用ByteBuffer的flip()方法
    - 读写使用不同的索引
    - 支持方法链式的调用
    - 支持引用计数
    - 支持池化

### ByteBuf如何工作的

!!! tip "ByteBuf如何工作的"

    维护了两个索引，一个用于读，一个用于写。随着写入或者读取，对应的索引指针会移动。
    
    如果读指针的值达到了写指针一样的值，会下标越界。
    
    read或write开头的方法，会推进指针，set或get不会，可指定最大容量。

### ByteBuf的使用模式

!!! tip "ByteBuf的使用模式"

    - [x] 1. 堆缓冲区或支撑数组
    
    数据存在jvm堆空间，适用于直接访问数组数据。
    
    - [x] 2. 直接缓冲区
    
    数据在堆外，适合网络数据传输，因为传输之前不需要再复制到直接缓冲区了。缺点是，如果要访问数组数据，还需要再复制到堆中。
    
    - [x] 3. 复合缓冲区
    
    将多个ByteBuf聚合，添加或删除。消除了不必要的复制，还提供了通用的API接口。


### ByteBuf的字节读写操作

!!! tip "ByteBuf的字节读写操作"

    - [x] 1. 随机访问索引
    
    第一个字节的索引是0，最后一个是capacity()-1，跟数组一样。使用一个索引值的方法，不会移动读写索引的指针，只有readerIndex(i)或者writeIndex(1)才可以。
    
    
    - [x] 2. 顺序访问索引
    
    分成三部分
    
    - 0~readerIndex：可丢弃字节（已被度过）
    - readerIndex~writerIndex：可读字节（未读）
    - writerIndex~capacity：可写字节
    
    不需要像ByteBuffer调flip方法进行读写切换。
    
    - [x] 3. 可丢弃字节
    
    discardReadBytes()方法丢弃，移动可读字节以及写索引，产生复制，慎用，只有内存宝贵的时候适合用。
    
    - [x] 4. 可读字节
    
    存储了实际数据，可读字节耗尽仍读取会数组下标越界。
    
    最好用buffer.isReadable()方法先判断
    
    - [x] 5. 可写字节
    
    未定义内容，写入就绪的内存区域。
    
    
### discardReadBytes()和clear()

!!! tip "discardReadBytes()和clear()"

        clear会将读写索引都设置为0，但是更轻量，它只重置索引不会产生复制。
        
    
### ByteBuf查找指定值的索引

!!! tip "ByteProcessor"

    确定指定值的索引
    
    int i = byteBuf.forEachByte(ByteProcessor.FIND_NUL);
        

### ByteBuf的duplicate()和copy()方法区别

!!! tip "ByteBuf的duplicate()和copy()方法区别"

    duplicate 读写索引是独立的，但是内部存储是共享的，修改了它的内容也会影响源实例
    copy 复制的是真实的副本


### ByteBufAllocator

!!! tip "ByteBufAllocator"

    池化，降低分配和释放内存的开销。
    
    两种实现：
    
    - PooledByteBufAllocator：默认的，池化的方式
    - UnpooledByteBufAllocator：非池化，每次调用返回新的实例
    
### Unpooled

!!! tip "Unpooled"

    静态方法获取未池化的ByteBuf实例


### ByteBufUtil类

!!! tip " ByteBufUtil类"

    
    equals，compare方法比较
    
    hexdump()打印调试的日志


### 引用计数

!!! tip "引用计数"

    标记对象不再被其他对象引用，降低内存的开销。
    
    - refCnt:计数值
    - release:释放引用计数的对象


### ChannelHandler生命周期

!!! tip "ChannelHandler生命周期"

    - handlerAdded: 当把ChannelHandler添加到ChannelPipeline中时调用
    - handlerRemoved: 从中移除时调用
    - exceptionCaught: 当处理过程中在ChannelPipeline有错误产生时调用


### ChannelInboundHandler

!!! tip "ChannelInboundHandler"
    
    处理入站数据已经各种状态变化
    
    - ChannelInboundHandlerAdapter: 重写channelRead方法，需要手动释放ByteBuf实例
    - SimpleChannelInboundHandler: 不需要手动释放实例


### ChannelOutboundHandler

!!! tip "ChannelOutboundHandler"

    处理出站数据并且允许拦截所有的操作
    
    可以推迟操作或事件