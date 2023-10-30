- 什么是指令重排序
就是程序运行的顺序未必是你编码的顺序

1. JVM指令重排
2. CPU指令并行

- happens-before 原则是什么？为什么需要 happens-before 原则？happens-before 常见规则有哪些？谈谈你的理解？

分布式系统中，使用一些列规则来定义事件的先手顺序，这个顺序不是时间的时钟顺序，而是逻辑顺序。

为了平衡程序员和处理器、编译器

程序员追求易于理解，强内存模型
处理器追求的是性能，弱内存模型

1. 传递规则
2. 线程start
3. 程序规则
4. volatile规则
5. 解锁规则

- 并发编程三个重要特性

原子性：一系列操作要么全部执行成功，要么全都不执行
有序性：禁止指令重排
可见性：并发情况下，一个线程对它的修改，对其它读取它的线程可见

- 如何保证变量的可见性？
volatile
- 如何禁止指令重排序？
volatile
- volatile 可以保证原子性么?
不能
- 乐观锁和悲观锁
悲观锁：假设最坏的情况，请求某个共享资源，一定会发生冲突，所以每次请求都要上锁，使用完再解锁
乐观锁：假设最好的情况，请求某个共享资源，一定不会发生冲突，所以不上锁，而是在真正修改之前，判断一下该资源是否被修改过，如果修改过，放弃修改，未修改过，使用CAS操作进行修改。
- 如何实现乐观锁？

版本号机制

CAS算法

v：原始值
n：新值
e：期望值

仅v==e时，才将v更新成n


- 乐观锁存在哪些问题？

ABA问题
长期自旋，cpu开销大
只能解决单个变量的原子操作

- synchronized 是什么？有什么用？

一个关键字，用来保证一组共享资源，被顺序读取

- 如何使用 synchronized？

修饰实例方法
修饰类静态方法
修饰代码款（类/方法）

- 构造方法可以用 synchronized 修饰么？
不可以
- synchronized 底层原理了解吗？

修饰代码块，会在进入方法前，执行monitorenter指令，在方法结束执行monitorexit指令

修饰方法，会在方法的ACC_SYNCHRONIZED标识中，保存该方法为同步方法

本质上，两者都是对monitor对象的获取

- JDK1.6 之后的 synchronized 底层做了哪些优化？

自旋锁、偏向锁、轻量级锁、适应性自旋锁、锁消除、锁粗话等技术

- synchronized 和 volatile 有什么区别？

volatile更轻量
volatile只能修饰变量
synchronized修饰方法和代码块
volatile保证有序性和可见性，synchronized都能保证

- ReentrantLock 是什么？

一个可重入锁，独占锁，支持公平锁、等待可中断、可选择性通知等

- 公平锁和非公平锁有什么区别？

公平锁：先等待的，先获取锁
非公平：抢占式的，后来的可能先获取到锁

- synchronized 和 ReentrantLock 有什么区别？

都是可重入锁，独占锁
synchronized jvm团队基于底层的优化和实现，源码不可见
ReentrantLock 基于jdk，源码可见
synchronized只能修饰代码块和方法，ReentrantLock更灵活
ReentrantLock支持公平锁和非公平锁，synchronized只支持非公平锁
ReentrantLock等待锁获取可人为中断或设置等待时长，synchronized不可中断
synchronized通知要么全部通知，要么随机选择通知，ReentrantLock可选择性通知

- 可中断锁和不可中断锁有什么区别？


- ReentrantReadWriteLock 是什么？
读写锁
里面有两个锁，一个读锁，一个写锁
读锁是个共享锁，多个读可并发
写锁是独占锁
- ReentrantReadWriteLock 适合什么场景？
读多写少的
- 共享锁和独占锁有什么区别？

- 线程持有读锁还能获取写锁吗？

不可以

- 读锁为什么不能升级为写锁？

两个读锁同时要升级写锁，会导致死锁

- Atomic 原子类介绍，有哪几种类型，分别介绍一下?

基本类型

数组类型

引用类型

属性修改类型

- ThreadLocal 有什么用？
让线程管理它自己内部的私有的数据副本，多线程环境下，不会产生冲突
- ThreadLocal 原理了解吗？

Thread内部有个ThreadLocalMap，专属的用来缓存数据副本的，通过ThreadLocal可以获取并修改它，ThreadLocal相当于一个封装或者说引用

- ThreadLocal 内存泄露问题是怎么导致的？

ThreadLocalMap中 key是ThreadLocal，然而ThreadLocal是一个弱引用，ThreadLocal如果没有被外部强引用的情况下，可能会被jvm清除，这个map的key就为null，导致永远不会被清除

- 什么是线程池?为什么要用线程池？如何创建线程池？

管理一系列线程的资源池。

- 为什么不推荐使用内置线程池？
- 线程池常见参数有哪些？如何解释？
- 线程池的饱和策略有哪些？
- 线程池常用的阻塞队列有哪些？
- 线程池处理任务的流程了解吗？
- 如何给线程池命名？
- 如何设定线程池的大小？
- 如何动态修改线程池的参数？
- Future 类有什么用？
- Callable 和 Future 有什么关系？
- CompletableFuture 类有什么用？
- AQS 是什么？
- AQS 的原理是什么？
- Semaphore 有什么用？
- Semaphore 的原理是什么？
- CountDownLatch 有什么用？
- CountDownLatch 的原理是什么？
- 用过 CountDownLatch 么？什么场景下用的？
- CyclicBarrier 有什么用？
- CyclicBarrier 的原理是什么？