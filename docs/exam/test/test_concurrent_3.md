- 什么是指令重排序
就是程序执行的顺序，不一定是代码编写的顺序
jvm指令重排：在不改变单线程语义的情况下，jvm可能会改变代码的执行顺序
处理器指令并行：现代处理器的指令并行技术

- happens-before 原则是什么？为什么需要 happens-before 原则？happens-before 常见规则有哪些？谈谈你的理解？

分布式系统中，定义一系列的规则，用来表示逻辑时钟的顺序，这个逻辑时钟，不是真实的时间，而是定义了事件的先后发生顺序。

为了达到程序员和编译器或者处理器的一个平衡

程序员追求的是代码有序，强内存模型，规则，代码可理解
编译器和处理器追求的是性能，只要不改变最后的执行结果，它会随意的进行指令的重排调整，以达到最大的性能，是弱内存模型

传递规则：A hb B B hb C 那么A hb C
线程启动规则：start方法 hb所有其它操作
代码规则：先写的代码，hb 后写的
锁规则：解锁 hb 加锁

- 并发编程三个重要特性

原子性：一个操作或者一系列操作，要么全部执行成功，要么全都不执行
可见性：并发访问共享资源，如果某个线程对资源进行修改，要对其他并发读的线程可见，读的得是改了之后的值
有序性：多线程情况下，不论怎么执行，最后的结果是一样的

- 如何保证变量的可见性？

volatile 原子类 锁

- 如何禁止指令重排序？
volatile
- volatile 可以保证原子性么?
不能
- 乐观锁和悲观锁

悲观锁，每次假设最坏的情况，访问的资源一定会被修改，所以每次访问资源一定要上锁，等修改之后解锁。
乐观锁，每次假设最好的情况，访问的资源一定不会被修改，所以每次访问资源都不上锁，只有最后修改的时候判断资源是否被修改了，如果被改了就放弃，如果没被改就尝试cas修改

- 如何实现乐观锁？

版本号机制、时间戳

cas操作

- 乐观锁存在哪些问题？

aba问题，线程访问共享资源的时候，值被改过，又改回了原值，版本号能解决

长时间自旋，cpu消耗很大，如果修改写的情况下比较多，资源长时间处于被争抢，导致长时间的自旋

只能修饰单个变量


- synchronized 是什么？有什么用？


一个关键字，用来保证代码块或者方法的同步性，并发情况下，也会有序执行

- 如何使用 synchronized？

修饰方法，修饰代码块，修饰类静态方法

- 构造方法可以用 synchronized 修饰么？

不可以，也不用，天然线程安全
- synchronized 底层原理了解吗？

对monitor对象的获取

修饰方法的时候，会在方法的ACC_sync字段写入这个对象是个同步的上锁的状态

修饰代码块，使用monitorenter和monitorexit指令，方法开始执行之前，先执行monitorenter指令，执行完方法，执行monitorexit指令

- JDK1.6 之后的 synchronized 底层做了哪些优化？

自旋锁、偏向锁、轻量级锁、锁消除、锁粗话等

- synchronized 和 volatile 有什么区别？
volatile更轻量
volatile只能修饰单个变量，synchronized能修饰代码块和方法
volatile能保证有序性和可见性，synchronized能保证三者



- ReentrantLock 是什么？
可重入锁，独占锁，基于AQS开发的，支持公平锁，锁等待可中断、选择性通知等
- 公平锁和非公平锁有什么区别？

公平锁：锁的获取是排队的，先到的先得
非公平锁：争抢式的，后到也可能先得

- synchronized 和 ReentrantLock 有什么区别？

synchronized很多优化和特性，是jvm团队基于底层研发的，未暴漏，ReentrantLock是基于jdk研发的，可以看实现源码
都是独占锁和可重入锁
synchronized只支持非公平锁，ReentrantLock支持公平锁
ReentrantLock更灵活也更强大，还支持等待可中断，选择性通知等

- 可中断锁和不可中断锁有什么区别？

可中断：等待锁释放的过程可以被中断

- ReentrantReadWriteLock 是什么？
读写锁
里面包含两个锁，一个读锁，一个写锁

读锁是共享锁，写锁是独占锁，也是可重入锁

- ReentrantReadWriteLock 适合什么场景？

读多写少的场景


- 共享锁和独占锁有什么区别？

共享锁：可以被多个线程并发读取

- 线程持有读锁还能获取写锁吗？

不能

- 读锁为什么不能升级为写锁？

如果遇到两个读锁同时升级写锁，会导致死锁

- Atomic 原子类介绍，有哪几种类型，分别介绍一下?

基本类
AtomicInteger
AtomicLong
AtomicBoolean
数组类
AtomicLongArray
AtomicIntegerArray
AtomicReferenceArray

引用类

AtomicReference
AtomicStampedReference
AtomicTimestampReference

修改类
AtomicIntegerFieldUpdater
AtomicLongFieldUpdater
AtomicReferenceFieldUpdater

- ThreadLocal 有什么用？

让线程可以操作变量的副本，而不用担心线程安全问题

- ThreadLocal 原理了解吗？

每个线程都有个私有变量，ThreadLocalMap，key就是ThreadLocal本身，value值，ThreadLocal相当于对这个变量操作的封装，可以让线程管理这个私有变量

- ThreadLocal 内存泄露问题是怎么导致的？

key是弱引用，如果没有手动回收，可能会被jvm回收掉，导致nullkey，永远不会被回收掉了

- 什么是线程池?为什么要用线程池？如何创建线程池？

一个线程的资源池，最快的复用已创建好的线程，利用率高
减少创建和销毁线程的开销
便于监控
Executors
自定义线程池

- 为什么不推荐使用内置线程池？

内置线程池，队列或者线程池本身，容易oom
自定义线程池让程序员更能理解线程池的配置和运行规则，规避资源耗尽的风险

- 线程池常见参数有哪些？如何解释？

核心线程数：等待队列未满的情况下，最多能运行的线程数
最大线程数：等待队列已满，最多能运行的线程数
等待队列：核心线程数满了情况下，装新的任务
keepAliveTime：当大于核心线程数的线程空闲时，不会立刻销毁，会等待这个时间
unit：单位

- 线程池的饱和策略有哪些？

拒绝并抛异常
拒绝不通知
删掉最老的
交给执行线程本身自己去执行

- 线程池常用的阻塞队列有哪些？

Fixed singlon是无界的等待队列 linkedblockingqueue

cache用的syncQueue

delayed用的DelayedBlockingQueue，也是无界的

- 线程池处理任务的流程了解吗？

如果有空闲线程，直接执行
如果没有，
先判断是否达到核心线程数，如果没达到，创建新线程去执行
如果达到了，等待队列未满，先放入等待队列
如果等待队列满了，小于最大线程数，创建新线程去执行
如果大于最大线程数了，饱和策略

- 如何给线程池命名？

ThreadFactoryBuilder

自己实现ThreadFactory

- 如何设定线程池的大小？

如果是cpu密集型 cpu*2

如果是io密集型 cpu+1

- 如何动态修改线程池的参数？

线程池内置方法

或者用一些第三方的框架，美团的线程池

- Future 类有什么用？

一个接口，用来获取线程的执行结果

- Callable 和 Future 有什么关系？

FutureTask是对Callable的封装

- CompletableFuture 类有什么用？

更强大，支持异步获取执行结果不用阻塞，和多个线程的编排

- AQS 是什么？

一个抽象类，定义了对共享资源获取后上锁，锁释放唤醒等待线程的方法

- AQS 的原理是什么？

内部有个state值，用来表明是否上锁，state>0代表有锁，state=0代表无锁

如果线程访问共享资源，state通过cas操作，修改+1，其它访问的线程就会阻塞等待

阻塞等待用的是等待队列，其实是个虚拟的等待队列，用一个类，内部封装了线程的引用和锁的状态，以及他前后的前驱和后继的引用

- Semaphore 有什么用？

共享锁：可以让多个线程，并发访问资源

- Semaphore 的原理是什么？

内部也是AQS实现的，令牌就是AQS的state，当执行tryAcquire时候，令牌数通过cas-1，当令牌数<=0，阻塞后续的线程
release释放令牌，让令牌数+1，然后唤醒等待的线程

- CountDownLatch 有什么用？

让一组线程阻塞到一个屏障前，直到所有线程执行完，才会执行释放后面要执行的程序

- CountDownLatch 的原理是什么？

AQS，state就是构造器的值，每次countdown，state--，await后面的方法阻塞，当state==0，才会释放
- 用过 CountDownLatch 么？什么场景下用的？

用过，读取多个文件数据，最后统计

- CyclicBarrier 有什么用？

一堆线程都执行到达屏障之后，才会执行构造器里的后续逻辑

- CyclicBarrier 的原理是什么？

内部通过一个count作为计数器，每个线程到达，count--，当count=0，执行构造器里的逻辑