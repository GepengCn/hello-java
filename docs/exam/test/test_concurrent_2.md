- 什么是指令重排序

系统执行的指令的顺序不一定是按照代码编写的顺序

- 编译器指令重排：在不改变单线程语义的情况下，重新安排语句的执行顺序
- 处理器指令重排：现代处理器采用了指令并行技术来，如果不存在数据依赖，处理器可以改变指令的执行顺序

- 什么是主内存？什么是本地内存？

主内存：所有线程创建的实例对象都存放在主内存中
本地内存：每个线程都有一个私有的内存空间来保存共享变量的副本，每个线程只能访问自己的本地内存。


- happens-before 原则是什么？为什么需要 happens-before 原则？happens-before 常见规则有哪些？谈谈你的理解？

分布式系统中，有一套规则用来定义逻辑时钟的变化，从而能够对时间的先后顺序进行判断，逻辑时钟不度量时间本身，仅区分事件发生的前后顺序，其本质就是定义了一种happens-before


为了处理程序员和编译器、处理器的平衡

程序员追求的是易于理解和编程的强内存模型
编译器和处理器追求的较少约束的弱内存模型，让他们能尽可能的去优化性能

所以它的原则很简单

1. 为了对编译器和处理器的约束少，只要不改变程序的执行结果，怎么重排序优化都行
2. 对于程序员来说， 能够改变程序结果的重排序，必须被禁止

程序顺序规则：写在后面的代码 happens-before 写在前面的
解锁规则：解锁 happens-before 加锁
volatile规则：线程修改的结果必须对其它线程可见
传递规则：A happens-before B B happens-before C 那么 A 一定 happens-before C
线程启动规则：thread的start方法一定 happens-before 所有其他的线程动作


- 并发编程三个重要特性

原子性：一个操作或者多个操作要么全部执行，要么全都不会执行

可见性：线程对共享变量的修改对其它线程可见，或者看到的都是修改后的最新值

有序性：如果存在指令重排后，程序运行的结果放生变化的情况，会被禁止，这种情况就叫做有序性

- 如何保证变量的可见性？

volatile

- 如何禁止指令重排序？

volatile

- volatile 可以保证原子性么?

不能
- 乐观锁和悲观锁

悲观锁总是设想最差的情况，共享资源被访问的时候一定会出问题，会被修改，所以每次访问的时候都会上锁，这样其他线程想拿到资源就会被阻塞直到上一个线程把锁释放

synchronized reentrantlock

乐观锁总是设想最好的情况，认为共享资源被访问的时候一定不会被修改，所以不必上锁，只是在数据被修改的最后一步，判断数据是否被改动过，如果改动了就放弃修改，如果没改动则尝试改动。原子类是乐观锁的代表

- 如何实现乐观锁？

1. 版本号机制
增加一个提供一个version字段，将读取时的值与要修改前的version值比较下，只有一致的情况下才会真正修改，否则放弃
2. CAS算法

类似于版本号机制

数据的原值V 和对原值的期望值 相等的时候，才会将新值更新到原值上去


- 乐观锁存在哪些问题？

ABA问题

循环时间长开销大

只能保证一个共享变量的原子操作

- synchronized 是什么？有什么用？

是java中的一个关键字，主要是解决多个线程访问资源的同步性，可以保证被它修饰的方法或者代码块，在任意时刻只能有一个线程访问或者修改

- 如何使用 synchronized？

实例方法
类静态方法
代码块



- 构造方法可以用 synchronized 修饰么？
不能
- synchronized 底层原理了解吗？

修饰语句块的时候使用的是monitorenter和monitorexit，monitorenter指向代码块开始的位置，monitorexit指向代码块结束的位置。

修饰方法的时候用的是一个同步标识，用来指明该方法是同步方法

本质都是对对象监视器monitor的获取

- JDK1.6 之后的 synchronized 底层做了哪些优化？

偏向锁、轻量级锁、自旋锁、适应性自旋锁、锁消除、锁粗话等技术来减少锁的开销

- synchronized 和 volatile 有什么区别？

性能上，volatile要更轻量

volatile只能保证有序性和可见性，synchronized三者都能保证

volatile只能修饰变量，synchronized可以修饰代码块和方法

- ReentrantLock 是什么？

是一个可重入的独占锁，实现了Lock接口，跟synchronized很类似

不过它更灵活，增加了超时中断，手动中断，公平锁，可选性通知等特性

默认使用非公平锁，但是可以通过构造器显式指定公平锁

- 公平锁和非公平锁有什么区别？

公平锁：锁被释放时，先申请的先活的锁

非公平锁：抢占式的，后申请的可能先获得锁

- synchronized 和 ReentrantLock 有什么区别？
synchronized是jvm团队基于底层开发的，虚拟机团队做了很多优化，都是基于虚拟机层面实现的，没有向我们暴漏
ReentrantLock是基于jdk开发的，底层实现是开源的

ReentrantLock增加了很多特性，比如超时可中断，手动中断，可选择性通知，公平锁等

synchronized不能中断，只支持非公平锁

- 可中断锁和不可中断锁有什么区别？

线程获取锁的等待过程可以被人为中断，比如ReentrantLock
而synchronized不能被中断

- ReentrantReadWriteLock 是什么？
一个可重入读写锁，保证读的效率，限制写的并发

内部有两个锁，一个读锁 一个写锁

读锁是共享锁

写锁是独占锁

- ReentrantReadWriteLock 适合什么场景？

读多写少的场景

- 共享锁和独占锁有什么区别？

    共享锁：一把锁能被多个线程同时持有
    独占锁：一把锁只能被一个线程持有

- 线程持有读锁还能获取写锁吗？

不能

- 读锁为什么不能升级为写锁？

读锁升级为写锁会引起线程的争夺，毕竟写锁是独占锁，这样会影响性能

另外，还可能引起死锁的发生。比如两个读锁都想要升级为写锁，都需要对方释放自己的读锁，而双方都不释放，产生死锁

- Atomic 原子类介绍，有哪几种类型，分别介绍一下?

基础类型
AtomicInteger
AtomicLong
AtomicBoolean


数组类型

AtomicIntegerArray
AtomicLongArray
AtomicReferenceArray

引用类型

AtomicReference
AtomicStampedReference
AtomicMarkableReference

属性修改类型

AtomicIntegerFieldUpdater
AtomicLongFieldUpdater
AtomicReferenceFieldUpdater

- ThreadLocal 有什么用？

通常情况下，创建的变量是可以被任何线程修改的，如果要实现一个每个线程独有的属于自己的专属变量，就可以使用ThreadLocal

- ThreadLocal 原理了解吗？

线程的专属变量其实是在线程本身的ThreadLocalMap中，ThreadLocal可以理解为对ThreadLocalMap变量操作的封装，ThreadLocal通过Thread.currentThread()获取当前线程，然后就可以访问ThreadLocalMap变量，进而对其进行访问与操作

- ThreadLocal 内存泄露问题是怎么导致的？

ThreadLocalMap的key是ThreadLocal本身是一个弱引用，而value是强引用，如果ThreadLocal如果没有被外部强引用的情况下，会被虚拟机清理掉，这样就会出现key为null的情况，而value永远不会被清理掉

- 什么是线程池?为什么要用线程池？如何创建线程池？

管理一系列线程的资源池，当有任务来处理时，直接从线程池中获取线程来处理，处理完线程不会被立即销毁

降低资源消耗：通过复用已创建的线程降低创建和销毁线程造成的资源消耗
提高响应速度：有任务来的时候，直接复用线程池内的线程，不必等到创建好就能立即执行
提高线程的可管理性：便于对线程进行统一的管理、分配和监控

- 为什么不推荐使用内置线程池？

这样的处理方式能够让使用者明确线程运行的规则，规避资源耗尽的风险

- 线程池常见参数有哪些？如何解释？

核心线程数：等待队列未满的情况下，能够同时运行的最大线程数
最大线程数：等待队列已满的情况下，能够同时运行的最大线程数
等待队列：当核心线程数已满的时候，就会被放入队列中

keepTimeAlive:当大于核心线程数的线程空闲时，不会被立即销毁，而是会等待一段时间，才会被销毁
unit：keepTimeAlive单位
handler: 饱和策略

- 线程池的饱和策略有哪些？

1. 直接拒绝，抛弃+抛异常
2. 直接抛弃，抛弃
3. 直接抛弃最老的未处理的请求
4. 用调用线程池的线程直接处理该任务，如果该任务已经结束，则会直接抛弃

- 线程池常用的阻塞队列有哪些？

LinkedBlockingQueue：一个无界队列 SingleThreadExecutor FixedThreadPool

SynchronousQueue:等待队列，没有容量，不存储元素，目的保证提交的任务，有空闲线程，则启用空闲线程来处理，没有则创建新线程来处理

DelayedWorkQueue:延迟阻塞队列: ScheduledThreadPool和SingleThreadScheduledExecutor

内部的元素不是按照放入的时间排序，而是按照延迟的时间长短进行排序，内部采用的是堆的结构，可以保证每次出队的任务都是时间最靠前的

- 线程池处理任务的流程了解吗？

先判断是否达到核心线程数，如果没有达到，则创建新线程来处理新任务

如果达到核心线程数，等待队列未满，则放入等待队列

如果等待队列已满，但是未达到最大线程数，则继续创建新线程

如果已达到最大线程数，则交给饱和策略来处理

- 如何给线程池命名？

1. guava的ThreadFactoryBuilder

2. 自定义线程工厂，重写new线程的方法，在里面指定线程名称

- 如何设定线程池的大小？

看是cpu密集型还是io密集型

cpu密集型 n + 1

io密集型 2n

cpu密集型：大量计算，但是耗时较短，都是那种大数据排序等的

io密集型： 大量io处理，比如磁盘上的文件读取等，耗时较长

- 如何动态修改线程池的参数？

利用ThreadFactory提供的方法

Hippo 4j

Dynamic TP

- Future 类有什么用？

启用一个异步线程来获取线程执行的结果

- Callable 和 Future 有什么关系？

Future的实现类FutureTask 内部封装了Callable

- CompletableFuture 类有什么用？

相比于Future的阻塞获取结果，CompletableFuture提供了异步获取执行结果的方式，还有线程编排组合的方式

- AQS 是什么？

一个抽象类，用来构建锁和同步器

提供了一些通用功能的实现，ReentrantLock和Semphore

- AQS 的原理是什么？

如果请求的共享资源空闲，则将请求线程设置为工作线程。否则，提供一些列同步阻塞以及锁释放唤醒等待线程的机制

这个机制就是CLH队列实现的，CLH是一个虚拟的双向队列，它是将请求资源的线程封装成队列的一个节点，这个节点包含线程的引用，队列中的状态，前驱和后继

AQS用int型的state标识同步状态，用volatile修饰

另外state可以用getState setState compareAndSetState来操作

- Semaphore 有什么用？
允许多个线程并发访问共享资源

- Semaphore 的原理是什么？

内部用的AQS实现的，permits就是state，只有拿到令牌的才能执行

用acquire方法，先判断state或者permits是否大于0，大于0，则使用cas操作，将state--，如果<=0，则将线程放入阻塞队列等待唤醒

用release方法，使用cas操作将state++，然后唤醒阻塞队列里的线程，去重试acquire

- CountDownLatch 有什么用？

可以让多个线程阻塞在一个地方，直到多个线程都执行完毕，才执行后续的方法

- CountDownLatch 的原理是什么？

内部也是用AQS实现的，await方法是一个自旋操作，判断state值是否等于0

CountDownLatch构造器传入的值，就是state的值，每次countDown()，会使得state-1，当等于0时候，await方法就会放开阻塞，让后续的方法继续执行

- 用过 CountDownLatch 么？什么场景下用的？

用过，读取多个文件的结果，最后进行汇总解析计算

- CyclicBarrier 有什么用？

类似于CountDownLatch，同样可以让多个线程阻塞，直到每个线程都执行完，不同的是，CountDownLatch是一次性的

CyclicBarrier可以反复执行



- CyclicBarrier 的原理是什么？

传入的count值，每当一个线程执行完，调用await，会使得count--，直到count=0，才会执行构造器传入的方法