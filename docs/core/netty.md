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


### ChannelHandler适配器

!!! tip "ChannelHandler适配器"

    ChannelInboundHandlerAdapter和ChannelOutboundHandlerAdapter

    提供方法isSharable()。标注@Sharable注解，表明可以被添加到多个ChannelPipeline中，可以被多个Channel安全的共享

### 怎么检测内存泄露

!!! tip "怎么检测内存泄露"

    类ResourceLeakDetector，它将对应用程序的缓冲区分配做大约1%的采样来检测内存泄露，开销很小。

    四种检测级别，通过设置jvm启动选项`java -Dio.netty.leakDetectionLevel=ADVANCED`

    - DISABLED:禁用
    - SIMPLE:1%采样率，默认
    - ADVANCED:1%采样率，报告所有泄露以及访问位置
    - PARANOID:100%采样，报告所有泄露以及访问位置

### 怎么防止内存泄露

!!! tip "怎么防止内存泄露"

    - 手动通过ReferenceCountUtil.release(msg)释放
    - 使用SimpleChannelInboundHandler的channelRead0()，方法执行完，自动释放
    - 出站，除了release(msg)之外，还要promise.setSuccess()通知消息已被处理。
    - 如果消息到了实际的传输层，被写入或者Channel关闭时，会被自动释放

### 什么是ChannelPipeline

!!! tip "什么是ChannelPipeline"

    拦截流经Channel的入站和出站事件的ChannelHandler实例链

### 怎么修改ChannelPipeline

!!! tip "怎么修改ChannelPipeline"

    ChannelPipeline上的相关方法addFirst、addLast等新增或删除上面的ChannelHandler

### 有阻塞的业务怎么办

!!! tip "有阻塞的业务怎么办"


    使用DefaultEventExecutor来创建并发的线程池，如果某个ChannelHandler传入之后，就会从Channel本身的EventLoop种移除，不会阻塞整体，而是交给线程池来处理

    ```java
    final EventExecutorGroup slowTaskGroup = new DefaultEventExecutorGroup(asyncPoolSize);

    .addLast(slowTaskGroup, new CustomizedHandlerContext());
    ```


### 触发事件

!!! tip "触发事件"

    ChannelPipeline的API提供一系列的方法比如fire*、bind、connect等，它实际调用的是下一个ChannelHandler的对应方法


### ChannelPipeline传播事件

!!! tip "ChannelPipeline传播事件"

    传播时，会测试下一个ChannelHandler的类型是否和事件的运动方向匹配（出站还是入站）。不匹配会跳过。ChannelHandler也可以同时实现入站和出站接口。


### ChannelHandlerContext简介

!!! tip "ChannelHandlerContext简介"

    使得ChannelHandler可以和它的ChannelPipeline以及其他的ChannelHandler交互。

    可以通知它的下一个ChannelHandler，甚至动态修改它所属的ChannelPipeline编排。

    具有丰富的处理事件和执行 I/O 操作的API。


### ChannelHandlerContext与ChannelPipeline同样方法区别在哪

!!! tip "ChannelHandlerContext与ChannelPipeline同样方法区别在哪"

    - ChannelPipeline的方法沿着整个链路传播
    - ChannelHandlerContext会从关联的ChannelHandler开始，只会传播下一个处理该事件的ChannelHandler

### ChannelHandlerContext好处在哪

!!! tip "ChannelHandlerContext好处在哪"

    - ChannelHandlerContext和ChannelHandler关联（绑定）是永远不会变的，所以缓存对它的引用是安全的
    - ChannelHandlerContext调用的方法会产生更短的事件流（只传播下一个），所以性能更好


### ctx、Channel、ChannelHandler、Pipeline的关联

!!! tip "ctx、Channel、ChannelHandler、Pipeline的关联"


    - Channel：相当于移动的火车
    - ChannelHandler：就是途径的火车站
    - Pipeline：就是整个火车的线路
    - ctx：ChannelHandler添加到Pipeline时创建，管理它对应的ChannelHandler和下一个ChannelHandler的引用，相当于某个火车站管理员和对的下一站的指引员（传播给下一站消息）


### ctx什么应用

!!! tip "ctx什么应用"

    - 访问Channel
    - 访问ChannelPipeline


### 为什么想要从Pipeline某个特定点开始传播呢

!!! tip "为什么想要从Pipeline某个特定点开始传播呢"

    - 减少事件流经对它不感兴趣的ChannelHandler带来的开销
    - 避免事件流经对它不感兴趣的ChannelHandler


### 怎么从指定的ChannelHandler开始处理事件呢

!!! tip "怎么从指定的ChannelHandler开始处理事件呢"

    找到指定ChannelHandler之前的ctx


### ctx和ChannelHandler的高级用法

!!! tip "ctx和ChannelHandler的高级用法"


    - 通过ctx.pipeline()修改ChannelHandler,比如动态的协议切换
    - 缓存ctx的引用以供稍后使用，甚至不同的线程使用


### Sharable用法

!!! tip "Sharable用法"

    - 在多个ChannelPipeline中安装同一个ChannelHandler的一个常见原因是用于收集跨越多个Channel的统计信息
    - 最好保证@Sharable修饰的类里面是线程安全的，比如可以将方法上锁改成同步方法，或者有状态变量用原子类等等方式


### 处理入站异常

!!! tip "处理入站异常"

    - 要处理异常的ChannelHandler要重写exceptionCaught()方法
    - 异常也会按照入站方向移动，所以最好确保处理异常的ChannelHandler放在最后，保证异常总会被处理
    - 异常处理，可以关闭channel，或者尝试恢复重试等等，如果没做任何处理，Netty会waring日志记录，出现异常而且未被处理，并释放该异常。


### 处理出站异常

!!! tip "处理出站异常"


    - 无论正常完成还是异常，出站操作都会返回一个ChannelFuture，注册到上面的ChannelFutureListener会在操作完成后通知成功还是出错
    - 几乎所有ChannelOutboundHandler上的方法都会传入一个ChannelPromise的实例，是ChannelFuture的子类，用于异步通知，也可以立即通知。

### ChannelFuture和ChannelPromise区别

!!! tip "ChannelFuture和ChannelPromise区别"

    - 细致的异常处理用ChannelFuture
    - 一般的异常处理，ChannelPromise更简单


### ChannelOutboundHandler本身抛出异常会发生什么？

!!! tip "ChannelOutboundHandler本身抛出异常会发生什么？"

    Netty本身会通知任何已经注册到对应ChannelPromise的监听器