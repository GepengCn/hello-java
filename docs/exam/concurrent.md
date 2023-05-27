# 并发

## 思维导图

![](https://p.ipic.vip/iafdbt.jpg)

## 问题

- 什么是指令重排序
- happens-before 原则是什么？为什么需要 happens-before 原则？happens-before 常见规则有哪些？谈谈你的理解？
- 并发编程三个重要特性
- 如何保证变量的可见性？
- 如何禁止指令重排序？
- volatile 可以保证原子性么?
- 乐观锁和悲观锁
- 如何实现乐观锁？
- 乐观锁存在哪些问题？
- synchronized 是什么？有什么用？
- 如何使用 synchronized？
- 构造方法可以用 synchronized 修饰么？
- synchronized 底层原理了解吗？
- JDK1.6 之后的 synchronized 底层做了哪些优化？
- synchronized 和 volatile 有什么区别？
- ReentrantLock 是什么？
- 公平锁和非公平锁有什么区别？
- synchronized 和 ReentrantLock 有什么区别？
- 可中断锁和不可中断锁有什么区别？
- ReentrantReadWriteLock 是什么？
- ReentrantReadWriteLock 适合什么场景？
- 共享锁和独占锁有什么区别？
- 线程持有读锁还能获取写锁吗？
- 读锁为什么不能升级为写锁？
- Atomic 原子类介绍，有哪几种类型，分别介绍一下?
- ThreadLocal 有什么用？
- ThreadLocal 原理了解吗？
- ThreadLocal 内存泄露问题是怎么导致的？
- 什么是线程池?为什么要用线程池？如何创建线程池？
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

## 答案

### 什么是指令重排序
!!! tip "什么是指令重排序"

    简单来说就是系统在执行代码的时候并不一定是按照你写的代码的顺序依次执行。

    常见的指令重排序有下面 2 种情况：

    - 编译器优化重排：编译器（包括 JVM、JIT 编译器等）在不改变单线程程序语义的前提下，重新安排语句的执行顺序。

    - 指令并行重排：现代处理器采用了指令级并行技术来将多条指令重叠执行。

### appens-before 原则是什么？为什么需要 happens-before 原则？happens-before 常见规则有哪些？谈谈你的理解？
!!! tip "happens-before 原则是什么？为什么需要 happens-before 原则？happens-before 常见规则有哪些？谈谈你的理解？"

    - [x] happens-before 原则是什么？

    在分布式环境中，通过一系列规则来定义逻辑时钟的变化，从而能够对事件的先后顺序进行判断。逻辑时钟并不度量时间本身，仅区分事件发生的前后顺序，其本质就是定义了一种 happens-before 关系。

    - [x] 为什么需要 happens-before 原则？要点程序员和编译器的"：

        是为了程序员和编译器、处理器之间的平衡。

        程序员追求的是易于理解和编程的强内存模型，遵守既定规则编码即可。

        编译器和处理器追求的是较少约束的弱内存模型，让它们尽己所能地去优化性能，让性能最大化。

        happens-before 原则的设计思想其实非常简单：要点：

        - 为了对编译器和处理器的约束尽可能少，只要不改变程序的执行结果，编译器和处理器怎么进行重排序优化都行。
        - 对于会改变程序执行结果的重排序，JMM 要求编译器和处理器必须禁止这种重排序。

    - [x] happens-before 常见规则有哪些？谈谈你的理解？

        - 程序顺序规则：一个线程内，按照代码顺序，书写在前面的操作 happens-before 于书写在后面的操作；
        - 解锁规则：解锁 happens-before 于加锁；
        - volatile 变量规则：对一个 volatile 变量的写操作 happens-before 于后面对这个 volatile 变量的读操作。说白了就是对 volatile 变量的写操作的结果对于发生于其后的任何操作都是可见的。
        - 传递规则：如果 A happens-before B，且 B happens-before C，那么 A happens-before C；
        - 线程启动规则：Thread 对象的 start()方法 happens-before 于此线程的每一个动作。

### 并发编程三个重要特性
!!! tip "并发编程三个重要特性"

    - 原子性

    一次操作或者多次操作，要么所有的操作全部都得到执行并且不会受到任何因素的干扰而中断，要么都不执行。

    在 Java 中，可以借助synchronized、各种 Lock 以及各种原子类实现原子性。synchronized 和各种 Lock 可以保证任一时刻只有一个线程访问该代码块，因此可以保障原子性。各种原子类是利用 CAS (compare and swap) 操作（可能也会用到 volatile或者final关键字）来保证原子操作。

    - 可见性

    当一个线程对共享变量进行了修改，那么另外的线程都是立即可以看到修改后的最新值。在 Java 中，可以借助synchronized、volatile 以及各种 Lock 实现可见性。如果我们将变量声明为 volatile ，这就指示 JVM，这个变量是共享且不稳定的，每次使用它都到主存中进行读取。

    - 有序性

    由于指令重排序问题，代码的执行顺序未必就是编写代码时候的顺序。

    我们上面讲重排序的时候也提到过：

    > 指令重排序可以保证串行语义一致，但是没有义务保证多线程间的语义也一致 ，所以在多线程下，指令重排序可能会导致一些问题。

    在 Java 中，volatile 关键字可以禁止指令进行重排序优化。

### 如何保证变量的可见性？

!!! tip "如何保证变量的可见性？"

    在 Java 中，volatile 关键字可以保证变量的可见性，如果我们将变量声明为 volatile ，这就指示 JVM，这个变量是共享且不稳定的，每次使用它都到主存中进行读取。

    volatile 关键字能保证数据的可见性，但不能保证数据的原子性。synchronized 关键字两者都能保证。

### 如何禁止指令重排序？

!!! tip "如何禁止指令重排序？"

    在 Java 中，volatile 关键字除了可以保证变量的可见性，还有一个重要的作用就是防止 JVM 的指令重排序。 如果我们将变量声明为 volatile ，在对这个变量进行读写操作的时候，会通过插入特定的 内存屏障 的方式来禁止指令重排序。

### volatile 可以保证原子性么?

!!! tip "volatile 可以保证原子性么?"

    volatile 关键字能保证变量的可见性，但不能保证对变量的操作是原子性的。

### 乐观锁和悲观锁

!!! tip "乐观锁和悲观锁"

    - 什么是悲观锁？

        悲观锁总是假设最坏的情况，认为共享资源每次被访问的时候就会出现问题(比如共享数据被修改)，所以每次在获取资源操作的时候都会上锁，这样其他线程想拿到这个资源就会阻塞直到锁被上一个持有者释放。也就是说，共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程。

        像 Java 中synchronized和ReentrantLock等独占锁就是悲观锁思想的实现。

    - 什么是乐观锁？

        乐观锁总是假设最好的情况，认为共享资源每次被访问的时候不会出现问题，线程可以不停地执行，无需加锁也无需等待，只是在提交修改的时候去验证对应的资源（也就是数据）是否被其它线程修改了（具体方法可以使用版本号机制或 CAS 算法）。

        在 Java 中java.util.concurrent.atomic包下面的原子变量类（比如AtomicInteger、LongAdder）就是使用了乐观锁的一种实现方式 CAS 实现的。

        高并发的场景下，乐观锁相比悲观锁来说，不存在锁竞争造成线程阻塞，也不会有死锁的问题，在性能上往往会更胜一筹。但是，如果冲突频繁发生（写占比非常多的情况），会频繁失败和重试，这样同样会非常影响性能，导致 CPU 飙升。

    理论上来说：

    - 悲观锁通常多用于写比较多的情况下（多写场景，竞争激烈），这样可以避免频繁失败和重试影响性能，悲观锁的开销是固定的。不过，如果乐观锁解决了频繁失败和重试这个问题的话（比如LongAdder），也是可以考虑使用乐观锁的，要视实际情况而定。
    - 乐观锁通常多用于写比较少读比较多的情况下（多读场景，竞争较少），这样可以避免频繁加锁影响性能。不过，乐观锁主要针对的对象是单个共享变量（参考java.util.concurrent.atomic包下面的原子变量类）。

### 如何实现乐观锁？

!!! tip "如何实现乐观锁？"

    乐观锁一般会使用版本号机制或 CAS 算法实现，CAS 算法相对来说更多一些，这里需要格外注意

    - 版本号机制

    一般是在数据表中加上一个数据版本号 version 字段，表示数据被修改的次数。当数据被修改时，version 值会加一。当线程 A 要更新数据值时，在读取数据的同时也会读取 version 值，在提交更新时，若刚才读取到的 version 值为当前数据库中的 version 值相等时才更新，否则重试更新操作，直到更新成功。

    - CAS 算法

    CAS 的全称是 Compare And Swap（比较与交换） ，用于实现乐观锁，被广泛应用于各大框架中。CAS 的思想很简单，就是用一个预期值和要更新的变量值进行比较，两值相等才会进行更新。

    CAS 是一个原子操作，底层依赖于一条 CPU 的原子指令。

    CAS 涉及到三个操作数：

    - V：要更新的变量值(Var)
    - E：预期值(Expected)
    - N：拟写入的新值(New)

    当且仅当 V 的值等于 E 时，CAS 通过原子方式用新值 N 来更新 V 的值。如果不等，说明已经有其它线程更新了 V，则当前线程放弃更新。

### 乐观锁存在哪些问题？

!!! tip "乐观锁存在哪些问题？"

    - ABA 问题是乐观锁最常见的问题。

        如果一个变量 V 初次读取的时候是 A 值，并且在准备赋值的时候检查到它仍然是 A 值，那我们就能说明它的值没有被其他线程修改过了吗？很明显是不能的，因为在这段时间它的值可能被改为其他值，然后又改回 A，那 CAS 操作就会误认为它从来没有被修改过。这个问题被称为 CAS 操作的 "ABA"问题。

        ABA 问题的解决思路是在变量前面追加上版本号或者时间戳。JDK 1.5 以后的 AtomicStampedReference 类就是用来解决 ABA 问题的，其中的 compareAndSet() 方法就是首先检查当前引用是否等于预期引用，并且当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。

    - 循环时间长开销大

        CAS 经常会用到自旋操作来进行重试，也就是不成功就一直循环执行直到成功。如果长时间不成功，会给 CPU 带来非常大的执行开销。

    - 只能保证一个共享变量的原子操作

        CAS 只对单个共享变量有效，当操作涉及跨多个共享变量时 CAS 无效。但是从 JDK 1.5 开始，提供了AtomicReference类来保证引用对象之间的原子性，你可以把多个变量放在一个对象里来进行 CAS 操作.所以我们可以使用锁或者利用AtomicReference类把多个共享变量合并成一个共享变量来操作。


### synchronized 是什么？有什么用？

!!! tip "synchronized 是什么？有什么用？"

    synchronized 是 Java 中的一个关键字，主要解决的是多个线程之间访问资源的同步性，可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。


### 如何使用 synchronized？

!!! tip "如何使用 synchronized？"

    synchronized 关键字的使用方式主要有下面 3 种：

    1. 修饰实例方法（锁当前对象实例）：给当前对象实例加锁，进入同步代码前要获得 当前对象实例的锁 。
    2. 修饰静态方法（锁当前类）：给当前类加锁，会作用于类的所有对象实例 ，进入同步代码前要获得 当前 class 的锁。
    3. 修饰代码块（锁指定对象/类）：对括号里指定的对象/类加锁

        - synchronized(object) 表示进入同步代码库前要获得 给定对象的锁。
        - synchronized(类.class) 表示进入同步代码前要获得 给定 Class 的锁

### 构造方法可以用 synchronized 修饰么？

!!! tip "构造方法可以用 synchronized 修饰么？"

    构造方法不能使用 synchronized 关键字修饰。

    构造方法本身就属于线程安全的，不存在同步的构造方法一说。


### synchronized 底层原理了解吗？

!!! tip "synchronized 底层原理了解吗？"

    https://www.cnblogs.com/suger43894/p/12171341.html

    synchronized 同步语句块的实现使用的是 monitorenter 和 monitorexit 指令，其中 monitorenter 指令指向同步代码块的开始位置，monitorexit 指令则指向同步代码块的结束位置。

    synchronized 修饰的方法使用的是 ACC_SYNCHRONIZED 标识，该标识指明了该方法是一个同步方法。

    不过两者的本质都是对对象监视器 monitor 的获取。

### JDK1.6 之后的 synchronized 底层做了哪些优化？

!!! tip "JDK1.6 之后的 synchronized 底层做了哪些优化？"

    JDK1.6 对锁的实现引入了大量的优化，如偏向锁、轻量级锁、自旋锁、适应性自旋锁、锁消除、锁粗化等技术来减少锁操作的开销。

### synchronized 和 volatile 有什么区别？

!!! tip "synchronized 和 volatile 有什么区别？"

    - volatile 关键字是线程同步的轻量级实现，所以 volatile性能肯定比synchronized关键字要好。但是 volatile 关键字只能用于变量而 synchronized 关键字可以修饰方法以及代码块 。
    - volatile 关键字能保证数据的可见性和有序性，但不能保证数据的原子性。synchronized 关键字三者都能保证。

### ReentrantLock 是什么？

!!! tip "ReentrantLock 是什么？"

    ReentrantLock 实现了 Lock 接口，是一个可重入且独占式的锁，和 synchronized 关键字类似。

    不过，ReentrantLock 更灵活、更强大，增加了轮询、超时、中断、公平锁等高级功能。

    ReentrantLock 默认使用非公平锁，也可以通过构造器来显式的指定使用公平锁。

### 公平锁和非公平锁有什么区别？

!!! tip "公平锁和非公平锁有什么区别？"

    - 公平锁 : 锁被释放之后，先申请的线程先得到锁。性能较差一些，因为公平锁为了保证时间上的绝对顺序，上下文切换更频繁。
    - 非公平锁：锁被释放之后，后申请的线程可能会先获取到锁，是随机或者按照其他优先级排序的。性能更好，但可能会导致某些线程永远无法获取到锁。

### synchronized 和 ReentrantLock 有什么区别？

!!! tip "synchronized 和 ReentrantLock 有什么区别？"

    - [x] 1. 两者都是可重入锁

    可重入锁 也叫递归锁，指的是线程可以再次获取自己的内部锁。比如一个线程获得了某个对象的锁，此时这个对象锁还没有释放，当其再次想要获取这个对象的锁的时候还是可以获取的，如果是不可重入锁的话，就会造成死锁。

    JDK 提供的所有现成的 Lock 实现类，包括 synchronized 关键字锁都是可重入的。

    - [x] 2. synchronized 依赖于 JVM 而 ReentrantLock 依赖于 API

    synchronized 是依赖于 JVM 实现的，虚拟机团队在 JDK1.6 为 synchronized 关键字进行了很多优化，但是这些优化都是在虚拟机层面实现的，并没有直接暴露给我们。

    ReentrantLock 是 JDK 层面实现的，所以我们可以通过查看它的源代码，来看它是如何实现的。


    - [x] 3. ReentrantLock 比 synchronized 增加了一些高级功能

    主要来说主要有三点：

    - 等待可中断
    - 可实现公平锁
    - 可实现选择性通知（锁可以绑定多个条件）

### 可中断锁和不可中断锁有什么区别？

!!! tip "可中断锁和不可中断锁有什么区别？"

    - 可中断锁：获取锁的过程中可以被中断，不需要一直等到获取锁之后 才能进行其他逻辑处理。ReentrantLock 就属于是可中断锁。
    - 不可中断锁：一旦线程申请了锁，就只能等到拿到锁以后才能进行其他的逻辑处理。 synchronized 就属于是不可中断锁。

### ReentrantReadWriteLock 是什么？

!!! tip "ReentrantReadWriteLock 是什么？"

    ReentrantReadWriteLock 实现了 ReadWriteLock ，是一个可重入的读写锁，既可以保证多个线程同时读的效率，同时又可以保证有写入操作时的线程安全。

    ReentrantReadWriteLock 其实是两把锁，一把是 WriteLock (写锁)，一把是 ReadLock（读锁）。读锁是共享锁，写锁是独占锁。读锁可以被同时读，可以同时被多个线程持有，而写锁最多只能同时被一个线程持有。

    ReentrantReadWriteLock 也支持公平锁和非公平锁，默认使用非公平锁，可以通过构造器来显示的指定。

### ReentrantReadWriteLock 适合什么场景？

!!! tip "ReentrantReadWriteLock 适合什么场景？"

    由于 ReentrantReadWriteLock 既可以保证多个线程同时读的效率，同时又可以保证有写入操作时的线程安全。因此，在读多写少的情况下，使用 ReentrantReadWriteLock 能够明显提升系统性能。

### 共享锁和独占锁有什么区别？

!!! tip "共享锁和独占锁有什么区别？"

    - 共享锁：一把锁可以被多个线程同时获得。
    - 独占锁：一把锁只能被一个线程获得。

### 线程持有读锁还能获取写锁吗？

!!! tip "线程持有读锁还能获取写锁吗？"

    - 持有读锁的情况下，不能取得写锁。
    - 持有写锁的情况下，可以获取读锁。

### 读锁为什么不能升级为写锁？

!!! tip "读锁为什么不能升级为写锁？"

    读锁升级为写锁会引起线程的争夺，毕竟写锁属于是独占锁，这样的话，会影响性能。

    另外，还可能会有死锁问题发生。假设两个线程的读锁都想升级写锁，则需要对方都释放自己锁，而双方都不释放，就会产生死锁。

### Atomic 原子类介绍，有哪几种类型，分别介绍一下?

!!! tip "Atomic 原子类介绍，有哪几种类型，分别介绍一下?"

    1. 基本类型

        - AtomicInteger
        - AtomicLong
        - AtomicBoolean

    2. 数组类型

        - AtomicIntegerArray
        - AtomicLongArray
        - AtomicReferenceArray

    3. 引用类型

        - AtomicReference: 引用类型原子类
        - AtomicMarkableReference:原子更新带有标记的引用类型
        - AtomicStampedReference:原子更新带有版本号的引用类型。可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。

    4. 对象的属性修改类型

        - AtomicIntegerFieldUpdater: 原子更新整型字段的更新器
        - AtomicLongFieldUpdater: 原子更新长整型字段的更新器
        - AtomicReferenceFieldUpdater: 原子更新引用类型里的字段

### ThreadLocal 有什么用？

!!! tip "ThreadLocal 有什么用？"

    通常情况下，我们创建的变量是可以被任何一个线程访问并修改的。如果想实现每一个线程都有自己的专属本地变量就可以使用ThreadLocal

    可以使用 get() 和 set() 方法来获取默认值或将其值更改为当前线程所存的副本的值，从而避免了线程安全问题。

### ThreadLocal 原理了解吗？

!!! tip "ThreadLocal 原理了解吗？"

    最终的变量是放在了当前线程的 ThreadLocalMap 中，并不是存在 ThreadLocal 上，ThreadLocal 可以理解为只是ThreadLocalMap的封装。 ThrealLocal 类中可以通过Thread.currentThread()获取到当前线程对象后，可以访问到该线程的ThreadLocalMap对象，进而对其进行查询或修改。



### ThreadLocal 内存泄露问题是怎么导致的？

!!! tip "ThreadLocal 内存泄露问题是怎么导致的？"

    ThreadLocalMap 中使用的 key 为 ThreadLocal 的弱引用，而 value 是强引用。所以，如果 ThreadLocal 没有被外部强引用的情况下，在垃圾回收的时候，key 会被清理掉，而 value 不会被清理掉。这样一来，ThreadLocalMap 中就会出现 key 为 null 的 Entry。假如我们不做任何措施的话，value 永远无法被 GC 回收，这个时候就可能会产生内存泄露。

    ThreadLocalMap 实现中已经考虑了这种情况，在调用 set()、get()、remove() 方法的时候，会清理掉 key 为 null 的记录。使用完 ThreadLocal方法后 最好手动调用remove()方法


### 什么是线程池?为什么要用线程池？如何创建线程池？

!!! tip "什么是线程池?为什么要用线程池？如何创建线程池？"

    线程池就是管理一系列线程的资源池。当有任务要处理时，直接从线程池中获取线程来处理，处理完之后线程并不会立即被销毁。

    池化技术的思想主要是为了减少每次获取资源的消耗，提高对资源的利用率

    - 降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。
    - 提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。
    - 提高线程的可管理性。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。

    创建线程池：

    - 方式一：通过ThreadPoolExecutor构造函数来创建（推荐）。
    - 方式二：通过 Executor 框架的工具类 Executors 来创建。


### 为什么不推荐使用内置线程池？

!!! tip "为什么不推荐使用内置线程池？"

    这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。

    虽然Executor提供的四个静态方法创建线程池，但是不建议去使用，因为这些方法创建的线程池，如果使用不当，可能会有OOM的风险。比如newFixedThreadPool和newSingleThreadExecutor的问题是堆积的请求处理队列可能会耗费非常大的内存，甚至OOM。newCachedThreadPool和newScheduledThreadPool的问题是线程数最大数是Integer.MAX_VALUE，可能会创建数量非常多的线程，甚至OOM。



### 线程池常见参数有哪些？如何解释？

!!! tip "线程池常见参数有哪些？如何解释？"

    ThreadPoolExecutor 3 个最重要的参数：

    - corePoolSize : 任务队列未达到队列容量时，最大可以同时运行的线程数量。
    - maximumPoolSize : 任务队列中存放的任务达到队列容量的时候，当前可以同时运行的线程数量变为最大线程数。
    - workQueue: 等待队列，如果当前运行的线程数量达到核心线程数，就会被存放在队列中。

    ThreadPoolExecutor其他常见参数 :

    - keepAliveTime:线程池中的线程数量大于核心线程数时，如果这时没有新的任务提交，核心线程数之外的线程不会立即销毁，而是会等待超过了keepAliveTime才会被回收销毁；
    - unit : keepAliveTime 参数的时间单位。
    - handler :饱和策略。

### 线程池的饱和策略有哪些？

!!! tip "线程池的饱和策略有哪些？"

    如果当前同时运行的线程数量达到最大线程数量并且队列也已经被放满了任务时，ThreadPoolTaskExecutor 定义一些策略:

    - ThreadPoolExecutor.AbortPolicy： 抛出 RejectedExecutionException来拒绝新任务的处理。
    - ThreadPoolExecutor.CallerRunsPolicy： 调用执行自己的线程运行任务，如果执行程序已关闭，则会丢弃该任务。
    - ThreadPoolExecutor.DiscardPolicy： 不处理新任务，直接丢弃掉。
    - ThreadPoolExecutor.DiscardOldestPolicy： 此策略将丢弃最早的未处理的任务请求。


### 线程池常用的阻塞队列有哪些？

!!! tip "线程池常用的阻塞队列有哪些？"

    - 容量为 Integer.MAX_VALUE 的 LinkedBlockingQueue（无界队列）：FixedThreadPool 和 SingleThreadExector 。由于队列永远不会被放满，因此FixedThreadPool最多只能创建核心线程数的线程。

    - SynchronousQueue（同步队列）：CachedThreadPool 。SynchronousQueue 没有容量，不存储元素，目的是保证对于提交的任务，如果有空闲线程，则使用空闲线程来处理；否则新建一个线程来处理任务。也就是说，CachedThreadPool 的最大线程数是 Integer.MAX_VALUE ，可以理解为线程数是可以无限扩展的，可能会创建大量线程，从而导致 OOM。

    - DelayedWorkQueue（延迟阻塞队列）：ScheduledThreadPool 和 SingleThreadScheduledExecutor 。DelayedWorkQueue 的内部元素并不是按照放入的时间排序，而是会按照延迟的时间长短对任务进行排序，内部采用的是“堆”的数据结构，可以保证每次出队的任务都是执行时间最靠前的。


### 线程池处理任务的流程了解吗？

!!! tip "线程池处理任务的流程了解吗？"

    ![](https://p.ipic.vip/fhsr06.jpg)

    1. 如果当前运行的线程数小于核心线程数，那么就会新建一个线程来执行任务。
    2. 如果当前运行的线程数等于或大于核心线程数，但是小于最大线程数，那么就把该任务放入到任务队列里等待执行。
    3. 如果向任务队列投放任务失败（任务队列已经满了），但是当前运行的线程数是小于最大线程数的，就新建一个线程来执行任务。
    4. 如果当前运行的线程数已经等同于最大线程数了，新建线程将会使当前运行的线程超出最大线程数，那么当前任务会被拒绝，饱和策略会调用RejectedExecutionHandler.rejectedExecution()方法。

### 如何给线程池命名？

!!! tip "如何给线程池命名？"

    初始化线程池的时候需要显示命名（设置线程池名称前缀），有利于定位问题。

    默认情况下创建的线程名字类似 pool-1-thread-n 这样的，没有业务含义，不利于我们定位问题。

    给线程池里的线程命名通常有下面两种方式：

    - 利用 `guava` 的 `ThreadFactoryBuilder`

    ```java
    ThreadFactory threadFactory = new ThreadFactoryBuilder()
    .setNameFormat(threadNamePrefix + "-%d")
    .setDaemon(true).build();
    ExecutorService threadPool = new ThreadPoolExecutor(corePoolSize, maximumPoolSize, keepAliveTime, TimeUnit.MINUTES, workQueue, threadFactory)

    ```

    - 自己实现 `ThreadFactory`。

    ```java
    import java.util.concurrent.Executors;
    import java.util.concurrent.ThreadFactory;
    import java.util.concurrent.atomic.AtomicInteger;
    /**
    * 线程工厂，它设置线程名称，有利于我们定位问题。
    */
    public final class NamingThreadFactory implements ThreadFactory {

        private final AtomicInteger threadNum = new AtomicInteger();
        private final ThreadFactory delegate;
        private final String name;

        /**
        * 创建一个带名字的线程池生产工厂
        */
        public NamingThreadFactory(ThreadFactory delegate, String name) {
            this.delegate = delegate;
            this.name = name; // TODO consider uniquifying this
        }

        @Override
        public Thread newThread(Runnable r) {
            Thread t = delegate.newThread(r);
            t.setName(name + " [#" + threadNum.incrementAndGet() + "]");
            return t;
        }

    }

    ```
### 如何设定线程池的大小？

!!! tip "如何设定线程池的大小？"

    - CPU 密集型任务(N+1)： 这种任务消耗的主要是 CPU 资源，可以将线程数设置为 N（CPU 核心数）+1。比 CPU 核心数多出来的一个线程是为了防止线程偶发的缺页中断，或者其它原因导致的任务暂停而带来的影响。一旦任务暂停，CPU 就会处于空闲状态，而在这种情况下多出来的一个线程就可以充分利用 CPU 的空闲时间。

    - I/O 密集型任务(2N)： 这种任务应用起来，系统会用大部分的时间来处理 I/O 交互，而线程在处理 I/O 的时间段内不会占用 CPU 来处理，这时就可以将 CPU 交出给其它线程使用。因此在 I/O 密集型任务的应用中，我们可以多配置一些线程，具体的计算方法是 2N。

    **如何判断是 CPU 密集任务还是 IO 密集任务？**

    CPU 密集型简单理解就是利用 CPU 计算能力的任务比如你在内存中对大量数据进行排序。但凡涉及到网络读取，文件读取这类都是 IO 密集型，这类任务的特点是 CPU 计算耗费时间相比于等待 IO 操作完成的时间来说很少，大部分时间都花在了等待 IO 操作完成上。

### 如何动态修改线程池的参数？

!!! tip "如何动态修改线程池的参数？"

    ThreadPoolExecutor 提供的下面这些方法。

    ![](https://p.ipic.vip/6l6qoq.jpg)

    格外需要注意的是corePoolSize， 程序运行期间的时候，我们调用 setCorePoolSize（）这个方法的话，线程池会首先判断当前工作线程数是否大于corePoolSize，如果大于的话就会回收工作线程。

    - Hippo-4：一款强大的动态线程池框架，解决了传统线程池使用存在的一些痛点比如线程池参数没办法动态修改、不支持运行时变量的传递、无法执行优雅关闭。除了支持动态修改线程池参数、线程池任务传递上下文，还支持通知报警、运行监控等开箱即用的功能。

    - Dynamic TP：轻量级动态线程池，内置监控告警功能，集成三方中间件线程池管理，基于主流配置中心（已支持 Nacos、Apollo，Zookeeper、Consul、Etcd，可通过 SPI 自定义实现）。


### Future 类有什么用？

!!! tip "Future 类有什么用？"

    简单理解就是：我有一个任务，提交给了 Future 来处理。任务执行期间我自己可以去做任何想做的事情。并且，在这期间我还可以取消任务以及获取任务的执行状态。一段时间之后，就可以 Future 那里直接取出任务执行结果。

### Callable 和 Future 有什么关系？

!!! tip "Callable 和 Future 有什么关系？"

    FutureTask相当于对Callable 进行了封装，管理着任务执行的情况，存储了 Callable 的 call 方法的任务执行结果。

### CompletableFuture 类有什么用？

!!! tip "CompletableFuture 类有什么用？"
        CompletableFuture相当于Future的增强版，比如可以异步获取结果，不必阻塞，除此之外还提供了函数式编程、异步任务编排组合（可以将多个异步任务串联起来，组成一个完整的链式调用）等能力。


### AQS 是什么？

!!! tip "AQS 是什么？"

    AQS 就是一个抽象类，主要用来构建锁和同步器。

    AQS 为构建锁和同步器提供了一些通用功能的是实现，比如 ReentrantLock，Semaphore。

### AQS 的原理是什么？

!!! tip "AQS 的原理是什么？"

    AQS 核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制 AQS 是用 CLH 队列实现的。

    CLH队列是一个虚拟的双向队列。AQS 是将请求共享资源的线程封装成队列的一个结点。节点中保存着线程的引用、在队列中的状态、前驱节点、后继节点。

    AQS 使用 int 成员变量 state 表示同步状态，由 volatile 修饰。

    另外，状态信息 state 可以通过getState()、setState()和compareAndSetState() 进行操作。

### Semaphore 有什么用？

!!! tip "Semaphore 有什么用？"

    synchronized 和 ReentrantLock 都是一次只允许一个线程访问共享资源，而Semaphore允许多个线程并发访问共享资源

    Semaphore 通常用于那些资源有明确访问数量限制的场景比如限流（仅限于单机模式，实际项目中推荐使用 Redis +Lua 来做限流）

### Semaphore 的原理是什么？

!!! tip "Semaphore 的原理是什么？"

    Semaphore 是共享锁的一种实现，它默认构造 AQS 的 state 值为 permits也就是令牌数，只有拿到令牌的线程才能执行。

    - 调用acquire() ，线程尝试获取令牌，如果 state >= 0 的话，则表示可以获取成功。然后使用 CAS 操作去修改 state 的值 state=state-1。如果 state<0 的话，则表示令牌数量不足，将线程加入阻塞队列，挂起当前线程。

    - 调用release()，线程尝试释放令牌，并使用 CAS 操作去修改 state 的值 state=state+1。释放令牌成功之后，同时会唤醒同步队列中的一个线程,被唤醒的线程会重新尝试令牌。


### CountDownLatch 有什么用？

!!! tip "CountDownLatch 有什么用？"

    CountDownLatch 允许多个线程阻塞在一个地方，直至所有线程的任务都执行完毕，才会执行后续的任务。

### CountDownLatch 的原理是什么？

!!! tip "CountDownLatch 的原理是什么？"

    CountDownLatch 是共享锁的一种实现,它默认构造 AQS 的 state 值为 count。

    当线程使用 countDown() 方法时,其实使用了tryReleaseShared方法以 CAS 的操作来减少 state,直至 state 为 0 。

    当调用 await() 方法的时候，如果 state 不为 0，那就证明任务还没有执行完毕，await() 方法就会一直阻塞。

    如果 state == 0 的话，就会释放所有等待的线程，await() 方法之后的语句得到执行。

### 用过 CountDownLatch 么？什么场景下用的？

!!! tip "用过 CountDownLatch 么？什么场景下用的？"

    之前在项目中，有一个使用多线程读取多个文件处理的场景，我用到了 CountDownLatch 。

    为此我们定义了一个线程池和的CountDownLatch对象。每一个线程处理完之后就执行countDown，await()方法后面是对汇总数据进行处理的逻辑。

    - 有没有可以改进的地方呢？

    可以使用 CompletableFuture 类来改进！异步、串行、并行或者等待所有线程执行完任务什么的都非常方便。

### CyclicBarrier 有什么用？

!!! tip "CyclicBarrier 有什么用？"

    CycliBarrier 是基于 ReentrantLock(ReentrantLock 也属于 AQS 同步器)和 Condition 的。

    让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。

### CyclicBarrier 的原理是什么？

!!! tip "CyclicBarrier 的原理是什么？"

    CyclicBarrier 内部通过一个 count 变量作为计数器，每当一个线程到了栅栏这里了，那么就将计数器减 1。如果 count 值为 0 了，表示这是这一代最后一个线程到达栅栏，就尝试执行构造方法中输入的任务。
