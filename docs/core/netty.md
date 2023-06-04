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


### EventLoop是什么？为什么要用它？

!!! tip "EventLoop是什么？为什么要用它？"

    - 运行任务来处理在连接的生命周期内发生的事件
    - 由一个永远不会改变的Thread驱动，任务可以直接提交给EventLoop实现，以立即执行或者调度执行


### EventLoop任务调度

!!! tip "EventLoop任务调度"


    ```java
    ctx.channel().eventLoop().schedule(new Runnable() {
        @Override
        public void run() {
            System.out.println("ln");
        }
    }, 60, TimeUnit.SECONDS);//60s后执行

    ctx.channel().eventLoop().scheduleAtFixedRate(new Runnable() {
        @Override
        public void run() {
            System.out.println("ln");
        }
    }, 60, 60, TimeUnit.SECONDS);//60s后开始，然后每60s执行一次

    ScheduledFuture<?> future = ctx.channel().eventLoop().scheduleAtFixedRate(...);

    future.cancel(true);//取消该任务

    ```


### EventLoop线程管理

!!! tip "EventLoop线程管理"

    - 如果调用线程正是支撑EventLoop的线程，那么代码会被直接执行
    - 否则，调用线程会调度该任务以便稍后执行，并把它放入队列，当支撑EventLoop线程空闲时，会优先处理队列里的任务。
    - 所以不要把耗时任务放入执行队列里，因为它会阻塞需要在同一线程上执行的任何其他任务。


### EventLoop异步传输

!!! tip "EventLoop异步传输"

    - EventLoopGroup负责为每个新创建的Channel分配一个EventLoop
    - 一旦一个Channel被分配给一个EventLoop，它将在它的整个生命周期都使用这个EventLoop
    - 因为一个EventLoop支撑多个Channel，所以对于所有相关联的Channel来说，ThreadLocal都将是一样的


### EventLoop阻塞传输

!!! tip "EventLoop阻塞传输"

    每个Channel都将被分配给**一个**EventLoop

### 什么是Bootstrap？

!!! tip "什么是Bootstrap？"

    - 对它进行配置，并使它运行起来的过程
    - 服务端：使用一个父Channel来接受来自客户端的连接，并创建子Channel用于它们之间的通信
    - 客户端：一个单独的、没有父Channel的Channel用于所有的网络交互（也适用于无连接的传输协议，比如UDP）


### 为什么引导类是Cloneable的

!!! tip "为什么引导类是Cloneable的"

    - 可能需要创建多个具有类似配置或完全相同配置的Channel
    - clone()方法
    - 只会创建EventLoopGroup的一个浅拷贝，可以在所有克隆的Channel实例之间共享
    - 典型场景：创建一个Channel进行一次HTTP请求

### 怎么引导客户端？

!!! tip "怎么引导客户端？"


    - 设置EventLoopGroup，提供用于处理Channel事件的EventLoop
    - channel(NioSocketChannel.class)：指定要使用的Channel实现
    - handler(...)：设置ChannelHandler用于处理事件和数据
    - connect()：连接到远程主机


### Channel和EventLoopGroup的兼容性

!!! tip "Channel和EventLoopGroup的兼容性"

    - 要一一对应，不能混用，比如NioEventLoopGroup对应NioSocketChannel、NioServerSocketChannel、NioDatagramChannel，OioEventLoopGroup对应OioSocketChannel...
    - 如果不对应，会抛异常IllegalStateException
    - 在调用bind()或connect()方法之前，必须调用group()、channel()或者Handler方法，不然也会抛异常IllegalStateException


### 引导服务器

!!! tip "引导服务器"

    - 设置EventLoopGroup，提供用于处理Channel事件的EventLoop
    - channel(NioSocketChannel.class)：指定要使用的Channel实现
    - childHandler(...)：设置子Channel的ChannelHandler用于处理事件和数据
    - bind()：配置绑定的端口


### 从Channel引导客户端

!!! tip "从Channel引导客户端"

    服务端充当客户端，连接另一个服务端，可以复用同一个eventLoop，避免额外的线程创建

    ```java

    public void channelActive(ctx){
        Bootstrap bootstrap = new Bootstrap();
        bootstrap.channel(NioSocketChannel.class).handler(...);
        bootstrap.group(ctx.channel().eventLoop());//复用当前Channel的eventLoop
    }

    ```


### 引导过程中添加多个ChannelHandler

!!! tip "引导过程中添加多个ChannelHandler"

    使用ChannelInitializer抽象类

    提供一种将多个ChannelHandler添加到一个ChannelPipeline中的简便方法


### ChannelOption

!!! tip "ChannelOption"

    常用于配置底层连接的详细信息

    ```java

    bootstrap.childOption(ChannelOption.SO_KEEPALIVE, Boolean.TRUE);

    ```


### 关闭Bootstrap

!!! tip "关闭Bootstrap"

    - 使用EventLoopGroup.shutdownGracefully()方法
    - 异步的操作，所以需要阻塞直到完成


### EmbeddedChannel概述

!!! tip "EmbeddedChannel概述"

    用于测试ChannelHandler，将入站或出站数据写入到EmbeddedChannel中，然后检查是否有任何东西达到了ChannelPipeline的尾端。以这种方式，确定消息是否被编解码过，以及是否触发了ChannelHandler的动作。


### 测试入站消息

!!! tip "测试入站消息"

    - new EmbeddedChannel([自定义ChannelHandler])
    - writeInbound()写入入站消息
    - readInbound()读取入站消息。入站消息穿越了整个pipeline。没有任何读取的，返回null。
    - finish() 将EmbeddedChannel标记为完成，如果有可读取的出入站数据返回true。这个方法还会调用EmbeddedChannel的close()方法。

### 测试出站消息

!!! tip "测试出站消息"

    - new EmbeddedChannel([自定义ChannelHandler])
    - writeOutbound()写入出站消息
    - readOutbound()读取出站消息。出站消息穿越了整个pipeline。没有任何读取的，返回null。
    - finish() 将EmbeddedChannel标记为完成，如果有可读取的出入站数据返回true。这个方法还会调用EmbeddedChannel的close()方法。


### 测试异常处理

!!! tip "测试异常处理"

    如果抛出了一个受检查的Exception，EmbeddedChannel会将它包装在一个RuntimeException中并被try/catch捕获；如果该类实现了exceptionCaught()方法并处理了该异常，则不会抛出被try/catch捕获。


### 什么是编解码器

!!! tip "什么是编解码器"

    编码器：将具有具体含义的结构化的数据转换为适用于传输的格式
    解码器：反之


### 编解码器中的引用计数

!!! tip "编解码器中的引用计数"

    - 一旦消息被编码或者解码，它就会被ReferenceCountUtil.release(message)调用自动释放。
    - 如果需要保留引用以便稍后使用， 可以调用ReferenceCountUtil.retain(message)。这回增加引用计数，防止该消息被释放。


### ByteToMessageDecoder

!!! tip "ByteToMessageDecoder"

    一个抽象类，将字节解码为消息

    ```java
    public class ToIntegerDecoder extends ByteToMessageDecoder {
        @Override
        protected void decode(ChannelHandlerContext channelHandlerContext, ByteBuf byteBuf, List<Object> list) throws Exception {
            if(byteBuf.readableBytes() >= 4) {//检查是否有4个字节可读（一个int的字节长度）
                list.add(byteBuf.readInt());
            }
        }
    }
    ```


### ReplayingDecoder

!!! tip "ReplayingDecoder"

    - 扩展了ByteToMessageDecoder，如果没有足够的字节可用，会抛出一个Error。如果足够的字节可供读取，该decode()方法会被再次调用
    - 相比于ByteToMessageDecoder稍慢一点，但是代码更简洁

    ```java
    public class ToIntegerDecoder extends ReplayingDecoder<Void> {
        @Override
        protected void decode(ChannelHandlerContext channelHandlerContext, ByteBuf byteBuf, List<Object> list) throws Exception {
            list.add(byteBuf.readInt());
        }
    }
    ```


### MessageToMessageDecoder

!!! tip "MessageToMessageDecoder"

    两个消息格式之间进行转换

    ```java
    public class IntegerToStringDecoder extends MessageToMessageDecoder<Integer> {
        @Override
        protected void decode(ChannelHandlerContext channelHandlerContext, Integer integer, List<Object> list) throws Exception {
            list.add(String.valueOf(integer));
        }
    }
    ```


### TooLongFrameException

!!! tip "TooLongFrameException"

    防止解码器缓冲的数据过大导致内存耗尽，抛出这个异常

    ```java
    public class SafeByteToMessageDecoder extends ByteToMessageDecoder {

        private static final int MAX_FRAME_SIZE = 1024;

        @Override
        protected void decode(ChannelHandlerContext channelHandlerContext, ByteBuf in, List<Object> out) throws Exception {
            int readable = in.readableBytes();
            if(readable > MAX_FRAME_SIZE){
                in.skipBytes(readable);
                throw new TooLongFrameException("Frame too big!");
            }
            //do something
            ...
        }
    }
    ```


### MessageToByteEncoder

!!! tip "MessageToByteEncoder"


    ```java
    public class ShortToByteEncoder extends MessageToByteEncoder<Short> {
        @Override
        protected void encode(ChannelHandlerContext channelHandlerContext, Short msg, ByteBuf out) throws Exception {
            out.writeShort(msg);
        }
    }
    ```


### MessageToMessageEncoder

!!! tip "MessageToMessageEncoder"

    ```java
    public class IntegerToStringEncoder extends MessageToMessageEncoder<Integer> {
        @Override
        protected void encode(ChannelHandlerContext channelHandlerContext, Integer msg, List<Object> out) throws Exception {
            out.add(String.valueOf(msg));
        }
    }
    ```


### ByteToMessageCodec

!!! tip "ByteToMessageCodec"

    ```java
    public class ShortToByteCodec extends ByteToMessageCodec<Short> {
        @Override
        protected void encode(ChannelHandlerContext channelHandlerContext, Short msg, ByteBuf out) throws Exception {
            out.writeShort(msg);
        }

        @Override
        protected void decode(ChannelHandlerContext channelHandlerContext, ByteBuf msg, List<Object> in) throws Exception {
            if (msg.readableBytes() >=2){
                in.add(msg.readShort());
            }
        }
    }
    ```


### MessageToMessageCodec

!!! tip "MessageToMessageCodec"

    ```java
    public class IntegerToStringCodec extends MessageToMessageCodec<Integer, String> {
        @Override
        protected void encode(ChannelHandlerContext ctx, String msg, List<Object> out) throws Exception {
            out.add(String.valueOf(msg));
        }

        @Override
        protected void decode(ChannelHandlerContext ctx, Integer msg, List<Object> in) throws Exception {
            in.add(String.valueOf(msg));
        }
    }
    ```


### CombinedChannelDuplexHandler

!!! tip "CombinedChannelDuplexHandler"

    结合编码器和解码器，编码器和解码器还可以单独部署，既灵活还不影响可重用性。

    ```java
    public class CombinedIntegerToStringCodec extends CombinedChannelDuplexHandler<IntegerToStringDecoder, IntegerToStringEncoder> {
        protected CombinedIntegerToStringCodec() {
            super(new IntegerToStringDecoder(), new IntegerToStringEncoder());
        }
    }
    ```


### SSL/TLS

!!! tip "SSL/TLS"



    ```java
    public class SslChannelInitializer extends ChannelInitializer<Channel> {
        private final SslContext context;
        private final boolean startTls;
        public SslChannelInitializer(SslContext context, boolean startTls) {
            this.context = context;
            this.startTls = startTls;
        }

        @Override
        protected void initChannel(Channel channel) throws Exception {
            SSLEngine engine = context.newEngine(channel.alloc());
            channel.pipeline().addFirst("ssl",new SslHandler(engine, startTls));
        }
    }
    ```


### HTTP编码器、解码器和编解码器

!!! tip "HTTP编码器、解码器和编解码器"


    - HttpRequestEncoder:将HttpRequest、HttpContent和LastHttpContent消息编码为字节
    - HttpResponseEncoder:将HttpResponse、HttpContent和LastHttpContent消息编码为字节
    - HttpRequestDecoder:将字节解码为HttpRequest、HttpContent和LastHttpContent
    - HttpResponseDecoder:将字节解码为HttpResponse、HttpContent和LastHttpContent

    ```java
    public class HttpPipelineInitializer extends ChannelInitializer<Channel> {

        private final boolean client;

        public HttpPipelineInitializer(boolean client) {
            this.client = client;
        }
        @Override
        protected void initChannel(Channel channel) throws Exception {
            ChannelPipeline pipeline = channel.pipeline();
            if(client){
                pipeline.addLast("decoder", new HttpResponseDecoder());//处理服务端的响应
                pipeline.addLast("encoder", new HttpRequestEncoder());//向服务端发送请求
            } else {
                pipeline.addLast("decoder", new HttpRequestDecoder());//接收来自客户端的请求
                pipeline.addLast("encoder", new HttpResponseEncoder());//向客户端发送相应
            }
        }
    }
    ```

    简化版本

    ```java
    public class HttpCodecInitializer extends ChannelInitializer<Channel> {

        private final boolean client;
        public HttpCodecInitializer(boolean client) {
            this.client = client;
        }
        @Override
        protected void initChannel(Channel channel) throws Exception {
            ChannelPipeline pipeline = channel.pipeline();
            if(client){
                pipeline.addLast("codec",new HttpServerCodec());
            } else {
                pipeline.addLast("codec",new HttpClientCodec());
            }
        }
    }
    ```


### 聚合HTTP消息

!!! tip "聚合HTTP消息"

    ```java
    pipeline.addLast("aggregator", new HttpObjectAggregator(512*1024));//将最大为512KB的消息的HttpObjectAggregator添加到pipeline
    ```

### HTTP压缩

!!! tip "HTTP压缩"

    ```java
    public class HttpCompressionInitializer extends ChannelInitializer<Channel> {
        private final boolean client;
        public HttpCompressionInitializer(boolean client) {
            this.client = client;
        }
        @Override
        protected void initChannel(Channel channel) throws Exception {
            ChannelPipeline pipeline = channel.pipeline();
            if(client){
                pipeline.addLast("codec", new HttpClientCodec());
                pipeline.addLast("decompressor", new HttpContentDecompressor());//解压缩
            } else {
                pipeline.addLast("codec", new HttpServerCodec());
                pipeline.addLast("compressor", new HttpContentCompressor());//压缩
            }
        }
    }
    ```


### 使用HTTPS

!!! tip "使用HTTPS"

    ```java
    public class HttpsCodecInitializer extends ChannelInitializer<Channel> {
        private final SslContext context;

        private final boolean isClient;

        public HttpsCodecInitializer(SslContext context, boolean isClient) {
            this.context = context;
            this.isClient = isClient;
        }

        @Override
        protected void initChannel(Channel channel) throws Exception {
            ChannelPipeline pipeline = channel.pipeline();
            SSLEngine engine = context.newEngine(channel.alloc());
            pipeline.addLast("ssl", new SslHandler(engine));
            if(isClient){
                pipeline.addLast("codec", new HttpClientCodec());
            } else {
                pipeline.addLast("codec", new HttpServerCodec());
            }
        }
    }

    ```


### 使用WebSocket

!!! tip "使用WebSocket"

    - 先建立HTTP通信
    - 然后发起WebSocket握手
    - 可以WebSocket通信
    - 如果要保护WebSocket，只需要把SslHandler作为第一个ChannelHandler添加到Pipeline中

    ```java
    public class WebSocketServerInitializer extends ChannelInitializer<Channel> {
        @Override
        protected void initChannel(Channel ch) throws Exception {
            ch.pipeline().addLast(
            new HttpServerCodec(),
            new HttpObjectAggregator(65536),
            new WebSocketServerProtocolHandler("/ws"),
            new TextFrameHandler()
            );
        }

        public static final class TextFrameHandler extends SimpleChannelInboundHandler<TextWebSocketFrame> {
            @Override
            protected void channelRead0(ChannelHandlerContext channelHandlerContext, TextWebSocketFrame textWebSocketFrame) throws Exception {
                //Handle text frame
            }
        }
    }
    ```



### 空闲的连接和超时

!!! tip "空闲的连接和超时"

    - IdleStateHandler:当连接空闲时间太长，将会触发一个IdleStateEvent事件。然后可以通过在ChannelInboundHandler中重写userEventTriggered()方法来处理。
    - ReadTimeoutHandler:如果在指定的时间间隔内没有收到任何的入站数据，则会抛出一个ReadTimeoutException并关闭对应的Channel。可以通过重写exceptionCaught()方法来检测ReadTimeoutException。
    - WriteTimeoutHandler:如果在指定的时间间隔内没有任何的出站数据写入，则会抛出一个WriteTimeoutException并关闭对应的Channel。可以通过重写exceptionCaught()方法来检测WriteTimeoutException。

    ```java
    public class IdleStateHandlerInitializer extends ChannelInitializer<Channel> {
        @Override
        protected void initChannel(Channel channel) throws Exception {
            ChannelPipeline pipeline = channel.pipeline();
            pipeline.addLast(new IdleStateHandler(0, 0, 60, TimeUnit.SECONDS));
            pipeline.addLast(new HeartbeatHandler());

        }
        public static final class HeartbeatHandler extends ChannelInboundHandlerAdapter {
            private static final ByteBuf HEARTBEAT_SEQUENCE = Unpooled.unreleasableBuffer(Unpooled.copiedBuffer("HEARTBEAT", CharsetUtil.ISO_8859_1));
            @Override
            public void userEventTriggered(ChannelHandlerContext ctx, Object evt) throws Exception {
                if(evt instanceof IdleStateEvent) {
                    ctx.writeAndFlush(HEARTBEAT_SEQUENCE.duplicate()).addListener(ChannelFutureListener.CLOSE_ON_FAILURE);//发送心跳，并且在心跳发送失败时关闭连接释放资源
                } else {
                    super.userEventTriggered(ctx, evt);//如果不是IdleStateEvent事件，则传递给下一个ChannelHandler
                }
            }
        }
    }
    ```