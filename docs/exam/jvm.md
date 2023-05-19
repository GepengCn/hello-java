# Java 内存区域详解 JVM

## 问题

- 运行时数据区域概述
- 程序计数器
- Java 虚拟机栈
- 程序运行中栈可能会出现的错误
- 本地方法栈
- 堆
- 堆可能出现的错误
- 方法区
- 方法区和永久代以及元空间是什么关系呢？
- 为什么要将永久代替换为元空间呢?
- 运行时常量池
- 字符串常量池
- JDK 1.7 为什么要将字符串常量池移动到堆中？
- 直接内存
- 直接内存怎么在java中应用的
- Java 对象的创建过程
- 内存分配的两种方式
- 内存分配并发问题
- 对象的内存布局
- 对象的访问定位
- 内存分配和回收原则
- 主要进行 gc 的区域
- 空间分配担保
- 死亡对象判断方法
- 哪些对象可以作为 GC Roots 呢？
- 对象可以被回收，就代表一定会被回收吗？
- 引用类型有哪些
- 如何判断一个类是无用的类？
- 垃圾收集算法有哪些？
- 垃圾收集器有哪些？
- 类加载器介绍
- 双亲委派模型介绍
- 双亲委派模型的执行流程
- 双亲委派模型的好处
- 打破双亲委派模型方法
- 显式指定堆内存
- 显式新生代内存
- 显式指定永久代/元空间的大小
- 指定垃圾回收垃圾器
- GC 日志记录
- 处理 OOM 配置
- 查看所有 Java 进程
- 监视虚拟机各种运行状态信息
- 实时地查看和调整虚拟机各项参数
- 生成堆转储快照
- 分析 heapdump 文件
- 生成虚拟机当前时刻的线程快照
- JVM线上问题排查和性能调优案例


## 答案
### 运行时数据区域概述

!!! tip "运行时数据区域概述"

    jdk1.8之前

    - 线程共享：堆、方法区、运行时常量池
    - 线程私有：虚拟机栈、本地方法栈、程序计数器
    - 本地内存：直接内存

    jdk1.8及之后

    - 线程共享：堆
    - 线程私有：虚拟机栈、本地方法栈、程序计数器
    - 本地内存：直接内存、元空间（方法区、运行时常量池）


### 程序计数器

!!! tip "程序计数器"

    当前线程所执行的字节码的行号指示器。字节码解释器通过改变这个计数器的值来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等功能都需要依赖这个计数器来完成。

    另外，为了线程切换后能恢复到正确的执行位置，每条线程都需要有一个独立的程序计数器，各线程之间计数器互不影响，独立存储。



### Java 虚拟机栈

!!! tip "Java 虚拟机栈"

    线程私有的，它的生命周期和线程相同，随着线程的创建而创建，随着线程的死亡而死亡。

    所有的 Java 方法调用都是通过栈来实现的

    方法调用的数据需要通过栈进行传递，每一次方法调用都会有一个对应的栈帧被压入栈中，每一个方法调用结束后，都会有一个栈帧被弹出。

    栈由一个个栈帧组成，而每个栈帧中都拥有：局部变量表、操作数栈、动态链接、方法返回地址。

    - **局部变量表**:主要存放了编译期可知的各种数据类型、对象引用
    - **操作数栈**:存放方法执行过程中产生的中间计算结果和临时变量。
    - **动态链接**:将符号引用转换为调用方法的直接引用 。
    - **方法返回地址**:一种是 return 语句正常返回，一种是抛出异常。不管哪种返回方式，都会导致栈帧被弹出。


### 程序运行中栈可能会出现的错误

!!! tip "程序运行中栈可能会出现的错误"

    - StackOverFlowError： 若栈的内存大小不允许动态扩展，那么当线程请求栈的深度超过虚拟机栈的最大深度的时候，就抛出 StackOverFlowError 错误。

    - OutOfMemoryError： 如果栈的内存大小可以动态扩展， 如果虚拟机在动态扩展栈时无法申请到足够的内存空间，则抛出OutOfMemoryError异常。

### 本地方法栈

!!! tip "本地方法栈"

    和虚拟机栈所发挥的作用非常相似，区别是：虚拟机栈为虚拟机执行 Java 方法 （也就是字节码）服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。 在 HotSpot 虚拟机中和 Java 虚拟机栈合二为一。


### 堆

!!! tip "堆"

    Java 虚拟机所管理的内存中最大的一块，所有线程共享的一块内存区域，在虚拟机启动时创建。**唯一目的就是存放对象实例**

    垃圾收集器管理的主要区域，因此也被称作 GC 堆。

### 堆可能出现的错误

!!! tip "堆可能出现的错误"

    - `java.lang.OutOfMemoryError: GC Overhead Limit Exceeded`:当jvm花太多时间执行垃圾回收但能回收很小的堆空间时
    - `java.lang.OutOfMemoryError: Java heap space`:创建对象时，堆剩余空间不足以存放的时候
### 方法区

!!! tip "方法区"

    存储已被虚拟机加载的 类信息、字段信息、方法信息、常量、静态变量、编译后的代码缓存等数据。


### 方法区和永久代以及元空间是什么关系呢？

!!! tip "方法区和永久代以及元空间是什么关系呢？"

    方法区和永久代以及元空间的关系很像 Java 中接口和类的关系，类实现了接口，这里的类就可以看作是永久代和元空间，接口可以看作是方法区，也就是说永久代以及元空间是 HotSpot 虚拟机对虚拟机规范中方法区的两种实现方式。


### 为什么要将永久代替换为元空间呢?

!!! tip "为什么要将永久代替换为元空间呢?"

    永久代有一个 JVM 本身设置的固定大小上限，无法进行调整，而元空间使用的是本地内存，受本机可用内存的限制，虽然元空间仍旧可能溢出，但是比原来出现的几率会更小。


### 运行时常量池

!!! tip "运行时常量池"

    用于存放编译期生成的各种字面量和符号引用。

### 字符串常量池

!!! tip "字符串常量池"

    为了提升性能和减少内存消耗专门开辟的一块区域，主要目的是避免字符串的重复创建。

    JDK1.7 之前，字符串常量池存放在永久代。JDK1.7 从永久代移动了 Java 堆中。

### JDK 1.7 为什么要将字符串常量池移动到堆中？

!!! tip "JDK 1.7 为什么要将字符串常量池移动到堆中？"

    因为永久代的 GC 回收效率太低，只有在Full GC的时候才会被回收。

### 直接内存

!!! tip "直接内存"

    不在 Java 堆或方法区中分配的，而是通过 JNI 的方式在本地内存上分配的。

### 直接内存怎么在java中应用的

!!! tip "直接内存怎么在java中应用的"

    NIO，引入了一种基于通道与缓冲区的 I/O 方式，它可以直接使用 JNI 直接分配堆外内存，然后通过存储在 Java 堆中的 DirectByteBuffer 对象来引用这块内存。在一些场景中显著提高性能，因为避免了在 Java 堆和 Native 堆之间来回复制数据。

### Java 对象的创建过程

!!! tip "Java 对象的创建过程或者类的生命周期"

    - [x] 加载

    - 通过全类名获取此类的二进制字节流。
    - 将静态存储结构转换为运行时数据结构。
    - 在内存中生成一个该类的对象，作为访问入口。

    - [x] 验证

    确保字节流中包含的信息符合《Java 虚拟机规范》，不会危害虚拟机自身的安全。

    - 文件格式检查
    - 语义检查
    - 类的正确性检查

    - [x] 准备

    为类的变量分配内存并设置初始值

    - 仅包括类变量，而不包括实例变量。实例变量会在对象实例化时分配。
    - final 关键字修饰的变量初始值设置多少就是多少，不会用初始值。


    - [x] 解析

    符号引用替换为直接引用的过程。

    - [x] 初始化

    执行初始化方法 <clinit> ()方法的过程，是类加载的最后一步，这一步才开始执行类中定义的程序代码。


    - [x] 使用和卸载

### 内存分配的两种方式(可选)

!!! tip "内存分配的两种方式"

    - 指针碰撞：
        - 适用场合：堆内存规整。
        - 原理：用过的内存全部整合到一边，没有用过的内存放在另一边，中间有一个分界指针，分配内存时，向没用过的内存区域移动指针。
        - GC 收集器：Serial, ParNew
    - 空闲列表：
        - 适用场合：堆内存不规整。
        - 原理：虚拟机会维护一个列表，记录哪些内存块是可用的，分配内存时，找一块儿足够大的内存块儿来划分给对象实例，最后更新列表记录。
        - GC 收集器：CMS

### 内存分配并发问题(可选)

!!! tip "内存分配并发问题"

    - CAS+失败重试
    - TLAB:为每一个线程预先在 Eden 区分配一块儿内存，JVM 在给线程中的对象分配内存时，首先在 TLAB 分配，当大于 TLAB 中的剩余内存，再采用上述的 CAS 进行内存分配

    什么事TLAB https://blog.csdn.net/hfer/article/details/106077631

### 对象的内存布局(可选)

!!! tip "对象的内存布局"

    分为 3 块区域：对象头、实例数据和对齐填充。

    Hotspot 虚拟机的对象头包括两部分信息，第一部分用于存储对象自身的运行时数据（哈希码、GC 分代年龄、锁状态标志等等），另一部分是类型指针，虚拟机通过这个指针来确定这个对象是哪个类的实例。

    实例数据，也就是在程序中定义的各种类型的字段内容。

    对齐填充部分不是必然存在的，也没有什么特别的含义，仅仅起占位作用。 因为 Hotspot 虚拟机的要求对象起始地址必须是 8 字节的整数倍，因此，当没有对齐时，就需要对齐填充来补全。


### 对象的访问定位(可选)

!!! tip "对象的访问定位"

    目前主流的访问方式有：使用句柄、直接指针。

    - [x] 句柄

        堆中将会划分出一块内存来作为句柄池，reference引用中存储的就是对象的句柄地址，而句柄中包含了对象实例与它的类型的具体地址信息。

    - [x] 直接指针

        如果使用直接指针访问，reference 中存储的直接就是对象的地址。

    HotSpot 虚拟机主要使用的就是**直接指针**方式来进行对象访问。

### 内存分配和回收原则

!!! tip "内存分配和回收原则"

    - [x] 对象优先在 Eden 区分配

    当 Eden 区没有足够空间进行分配时，虚拟机将发起一次 Minor GC

    - [x] 大对象直接进入老年代

    主要是为了避免为大对象分配内存时由于分配担保机制带来的复制而降低效率。

    - [x] 长期存活的对象将进入老年代

    如果对象在 Eden 出生并经过第一次 Minor GC 后仍然能够存活，并且能被 Survivor 容纳的话，将被移动到 Survivor 空间）中，并将对象年龄设为 1。

    对象在 Survivor 中每熬过一次 MinorGC,年龄就增加 1 岁，当它的年龄增加到一定程度（默认为 15 岁），就会被晋升到老年代中。


    - [x] 主要进行 gc 的区域

    针对 HotSpot VM 的实现，它里面的 GC 其实准确分类只有两大种：

    部分收集 (Partial GC)：

        - 新生代收集（Minor GC / Young GC）：只对新生代进行垃圾收集；
        - 老年代收集（Major GC / Old GC）：只对老年代进行垃圾收集。需要注意的是 Major GC 在有的语境中也用于指代整堆收集；
        - 混合收集（Mixed GC）：对整个新生代和部分老年代进行垃圾收集。

    整堆收集 (Full GC)：收集整个 Java 堆和方法区。


    - [x] 空间分配担保

    空间分配担保是为了确保在 Minor GC 之前老年代本身还有容纳新生代所有对象的剩余空间。担保成功minor GC， 否则Full GC

### 死亡对象判断方法

!!! tip "死亡对象判断方法"

    - [x] 引用计数法

    但是目前主流的虚拟机中并没有选择这个算法来管理内存，其最主要的原因是它很难解决对象之间循环引用的问题。

    - [x] 可达性分析算法

    这个算法的基本思想就是通过一系列的称为 “GC Roots” 的对象作为起点，从这些节点开始向下搜索，节点所走过的路径称为引用链，当一个对象到 GC Roots 没有任何引用链相连的话，则证明此对象是不可用的，需要被回收。

### 哪些对象可以作为 GC Roots 呢？

!!! tip "哪些对象可以作为 GC Roots 呢？"

    - 虚拟机栈(栈帧中的本地变量表)中引用的对象
    - 本地方法栈(Native 方法)中引用的对象
    - 方法区中类静态属性引用的对象
    - 方法区中常量引用的对象
    - 被同步锁持有的对象

### 对象可以被回收，就代表一定会被回收吗？

!!! tip "对象可以被回收，就代表一定会被回收吗？"

    当一个对象在可达性分析时被判定为不可达对象时，不会马上被回收，还需要进行两次标记。

    - 第一次标记：判断当前对象是否有 finalize() 方法并且该方法没有被执行过，若不存在则标记为垃圾对象，等待回收；若有的话，并且该方法没有被执行，则进行第二次标记；

    - 第二次标记将当前对象放入 F-Queue 队列，并生成一个线程去执行finalize方法，
如果执行了 finalize() 方法之后仍然没有与 GC Roots 有直接或者间接的引用，则该对象会被回收。



### 引用类型有哪些

!!! tip "引用类型有哪些"

    - [x] 强引用

    垃圾回收器绝不会回收它。当内存空间不足，Java 虚拟机宁愿抛出 OutOfMemoryError 错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。

    - [x] 软引用

    如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存
    - [x] 弱引用

    弱引用与软引用的区别在于：只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。

    - [x] 虚引用

    与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。

    **虚引用主要用来跟踪对象被垃圾回收的活动。**

### 如何判断一个类是无用的类？

!!! tip "如何判断一个类是无用的类？"

    - 该类所有的实例都已经被回收
    - 加载该类的 ClassLoader 已经被回收。
    - 该类没有在任何地方被引用，无法通过反射访问该类的方法。

### 垃圾收集算法有哪些？

!!! tip "垃圾收集算法有哪些？"

    - [x] 标记-清除算法

    首先标记出所有不需要回收的对象，在标记完成后统一回收掉所有没有被标记的对象。

    - 效率问题：标记和清除两个过程效率都不高。
    - 空间问题：标记清除后会产生大量不连续的内存碎片。

    - [x] 复制算法

    它可以将内存分为大小相同的两块，每次使用其中的一块。当这一块的内存使用完后，就将还存活的对象复制到另一块去，然后再把使用的空间一次清理掉。

    - 可用内存变小：可用内存缩小为原来的一半。
    - 不适合老年代：如果存活对象数量比较大，复制性能会变得很差。
    
    - [x] 标记-整理算法

    标记过程仍然与“标记-清除”算法一样，但后续步骤不是直接对可回收对象回收，而是让所有存活的对象向一端移动，然后直接清理掉端边界以外的内存。

    由于多了整理这一步，因此效率也不高，适合老年代这种垃圾回收频率不是很高的场景。


    - [x] 分代收集算法

    根据对象存活周期的不同将内存分为几块。一般将 Java 堆分为新生代和老年代

    比如在新生代中，每次收集都会有大量对象死去，所以可以选择”标记-复制“算法，只需要付出少量对象的复制成本就可以完成每次垃圾收集。

    而老年代的对象存活几率是比较高的，而且没有额外的空间对它进行分配担保，所以我们必须选择“标记-清除”或“标记-整理”算法进行垃圾收集。


### 垃圾收集器有哪些？

!!! tip "垃圾收集器有哪些？"

    - [x] Serial 收集器

    一个单线程收集器,进行垃圾收集工作的时候必须暂停其他所有的工作线程

    新生代采用标记-复制算法，老年代采用标记-整理算法。

    Serial 收集器由于没有线程交互的开销，自然可以获得很高的单线程收集效率。Serial 收集器对于运行在 Client 模式下的虚拟机来说是个不错的选择。


    - [x] ParNew 收集器

    其实就是 Serial 收集器的多线程版本


    - [x] Parallel Scavenge 收集器

    Parallel Scavenge 收集器关注点是吞吐量（高效率的利用 CPU）。CMS 等垃圾收集器的关注点更多的是用户线程的停顿时间（提高用户体验）。

    - [x] Serial Old 收集器

    Serial 收集器的老年代版本，它同样是一个单线程收集器。它主要有两大用途：一种用途是在 JDK1.5 以及以前的版本中与 Parallel Scavenge 收集器搭配使用，另一种用途是作为 CMS 收集器的后备方案。

    - [x] Parallel Old 收集器

    Parallel Scavenge 收集器的老年代版本。使用多线程和“标记-整理”算法。在注重吞吐量以及 CPU 资源的场合，都可以优先考虑 Parallel Scavenge 收集器和 Parallel Old 收集器。

    - [x] CMS 收集器

    一种以获取最短回收停顿时间为目标的收集器。它非常符合在注重用户体验的应用上使用。

    是一种 “标记-清除”算法实现的

    整个过程分为四个步骤：

    - **初始标记**:暂停所有的其他线程，并记录下直接与 root 相连的对象，速度很快 ；
    - **并发标记**:同时开启标记 和用户线程。
    - **重新标记**:为了修正并发标记期间因为用户程序运行而导致标记产生变动的记录，这个阶段的停顿时间一般会比初始标记阶段的时间稍长，远远比并发标记阶段时间短
    - **并发清除**:一边开启用户线程，一边对对未标记的对象做清除。

    - [x] G1 收集器
    面向服务器的垃圾收集器,既有较低的GC 停顿时间同时还具备高吞吐量。

    具备以下特点：

    - 并行与并发：G1 能充分利用CPU多核的优势来缩短停顿时间。
    - 分代收集：虽然 G1 可以不需要其他收集器配合就能独立管理整个 GC 堆，但是还是保留了分代的概念。
    - 空间整合：G1 从整体来看是基于“标记-整理”算法实现的收集器；从局部上来看是基于“标记-复制”算法实现的。
    - 可预测的停顿：能建立可预测的停顿时间模型，能让使用者指定消耗在垃圾收集上的时间。

    G1 收集器的运作大致分为以下几个步骤：

    - **初始标记**
    - **并发标记**
    - **最终标记**
    - **筛选回收**

    - [x] ZGC 收集器

    与 CMS 中的 ParNew 和 G1 类似，ZGC 也采用标记-复制算法，不过 ZGC 对该算法做了重大改进。在 ZGC 中出现 Stop The World 的情况会更少！Java11 的时候 ，ZGC 还在试验阶段。经过多个版本的迭代，不断的完善和修复问题，ZGC 在 Java 15 已经可以正式使用了！不过，默认的垃圾回收器依然是 G1。


### 类加载器介绍

!!! tip "类加载器介绍"

    类加载器的主要作用就是加载 Java 类的字节码到 JVM 中

    字节码可以是 Java 源程序（.java文件）经过 javac 编译得来，也可以是通过工具动态生成或者通过网络下载得来。

    JVM 中内置了三个重要的 ClassLoader：

    - BootstrapClassLoader(启动类加载器)：主要用来加载 JDK 核心类库

    - ExtensionClassLoader(扩展类加载器)：主要负责加载 %JRE_HOME%/lib/ext 目录下的 jar 包和类以及被 java.ext.dirs 系统变量所指定的路径下的所有类。

    - AppClassLoader(应用程序类加载器)：负责加载当前应用 classpath 下的所有 jar 包和类。

    - CustomClassLoader(自定义类加载器)：一般重写findClass方法即可，尽量别打破双亲委派模型。


### 双亲委派模型介绍

!!! tip "双亲委派模型介绍"

    - ClassLoader 类使用委托模型来搜索类和资源。
    - 双亲委派模型要求除了顶层的启动类加载器外，其余的类加载器都应有自己的父类加载器。
    - ClassLoader 实例会在试图亲自查找类或资源之前，将搜索类或资源的任务委托给其父类加载器。

### 双亲委派模型的执行流程

!!! tip "双亲委派模型的执行流程"

    - 在类加载的时候，系统会首先判断当前类是否被加载过。已经被加载的类会直接返回，否则才会尝试加载
    - 类加载器在进行类加载的时候，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成
    - 只有当父加载器反馈自己无法完成这个加载请求时，子加载器才会尝试自己去加载

### 双亲委派模型的好处

!!! tip "双亲委派模型的好处"

    双亲委派模型保证了 Java 程序的稳定运行，可以避免类的重复加载，也保证了 Java 的核心 API 不被篡改。

### 打破双亲委派模型方法

!!! tip "打破双亲委派模型方法"

    需要重写 loadClass() 方法

### 显式指定堆内存

!!! tip "显式指定堆内存"

    ```powershell
    -Xms2G //最小堆
    -Xmx5G //最大最
    ```

### 显式新生代内存
!!! tip "显式新生代内存"

    ```powershell
    第一种
    -XX:NewSize=256m //新生代分配 最小 256m 的内存
    -XX:MaxNewSize=1024m //新生代分配 最大 1024m
    第二种
    -Xmn256m //NewSize 与 MaxNewSize 设为一致
    ```


### 显式指定永久代/元空间的大小
!!! tip "显式指定永久代/元空间的大小"

    ```powershell
    -XX:MetaspaceSize=N //MetaspaceSize 表示 Metaspace 使用过程中触发 Full GC 的阈值，只对触发起作用。
    -XX:MaxMetaspaceSize=N #设置 Metaspace 的最大大小
    ```

### 指定垃圾回收垃圾器
!!! tip "指定垃圾回收垃圾器"

    ```powershell
    -XX:+UseSerialGC
    -XX:+UseParallelGC
    -XX:+UseParNewGC
    -XX:+UseG1GC
    ```

### GC 日志记录
!!! tip "GC 日志记录"

    ```powershell
    # 必选
    # 打印基本 GC 信息
    -XX:+PrintGCDetails
    -XX:+PrintGCDateStamps
    # 打印对象分布
    -XX:+PrintTenuringDistribution
    # 打印堆数据
    -XX:+PrintHeapAtGC
    # 打印Reference处理信息
    # 强引用/弱引用/软引用/虚引用/finalize 相关的方法
    -XX:+PrintReferenceGC
    # 打印STW时间
    -XX:+PrintGCApplicationStoppedTime

    # GC日志输出的文件路径
    -Xloggc:/path/to/gc-%t.log
    # 开启日志文件分割
    -XX:+UseGCLogFileRotation
    # 最多分割几个文件，超过之后从头文件开始写
    -XX:NumberOfGCLogFiles=14
    # 每个文件上限大小，超过就触发分割
    -XX:GCLogFileSize=50M
    ```

### 处理 OOM 配置
!!! tip "处理 OOM 配置"

    ```powershell
    -XX:+HeapDumpOnOutOfMemoryError
    -XX:HeapDumpPath=./java_pid<pid>.hprof
    -XX:OnOutOfMemoryError="< cmd args >;< cmd args >"
    -XX:+UseGCOverheadLimit
    ```

    - HeapDumpOnOutOfMemoryError 指示 JVM 在遇到 OutOfMemoryError 错误时将 heap 转储到物理文件中。
    - HeapDumpPath 表示要写入文件的路径; 可以给出任何文件名; 但是，如果 JVM 在名称中找到一个 <pid> 标记，则当前进程的进程 id 将附加到文件名中，并使用.hprof格式
    - OnOutOfMemoryError 用于发出紧急命令，以便在内存不足的情况下执行; 应该在 cmd args 空间中使用适当的命令。例如，如果我们想在内存不足时重启服务器，我们可以设置参数: -XX:OnOutOfMemoryError="shutdown -r" 。
    - UseGCOverheadLimit 是一种策略，它限制在抛出 OutOfMemory 错误之前在 GC 中花费的 VM 时间的比例#

### 查看所有 Java 进程
!!! tip "查看所有 Java 进程"

    ```powershell
    jps #显示虚拟机执行主类名称以及这些进程的本地虚拟机唯一 ID
    jps -q #只输出进程的本地虚拟机唯一 ID。
    jps -l #输出主类的全名，如果进程执行的是 Jar 包，输出 Jar 路径。
    jps -v #输出虚拟机进程启动时 JVM 参数。
    jps -m #输出传递给 Java 进程 main() 函数的参数。
    ```

### 监视虚拟机各种运行状态信息
!!! tip "监视虚拟机各种运行状态信息"

    ```powershell
    jstat -gc -h3 31736 1000 10   #表示分析进程 id 为 31736 的 gc 情况，每隔 1000ms 打印一次记录，打印 10 次停止，每 3 行后打印指标头部。
    ```

    - jstat -class vmid：显示 ClassLoader 的相关信息；
    - jstat -compiler vmid：显示 JIT 编译的相关信息；
    - jstat -gc vmid：显示与 GC 相关的堆信息；
    - jstat -gccapacity vmid：显示各个代的容量及使用情况；
    - jstat -gcnew vmid：显示新生代信息；
    - jstat -gcnewcapcacity vmid：显示新生代大小与使用情况；
    - jstat -gcold vmid：显示老年代和永久代的行为统计，从 jdk1.8 开始,该选项仅表示老年代，因为永久代被移除了；
    - jstat -gcoldcapacity vmid：显示老年代的大小；
    - jstat -gcutil vmid：显示垃圾收集信息；

### 实时地查看和调整虚拟机各项参数
!!! tip "实时地查看和调整虚拟机各项参数"

    jinfo vmid :输出当前 jvm 进程的全部参数和系统属性 (第一部分是系统的属性，第二部分是 JVM 的参数)。

    jinfo -flag name vmid :输出对应名称的参数的具体值。比如输出 MaxHeapSize、查看当前 jvm 进程是否开启打印 GC 日志 ( -XX:PrintGCDetails :详细 GC 日志模式，这两个都是默认关闭的)。

    ```powershell
    C:\Users\SnailClimb>jinfo  -flag MaxHeapSize 17340
    -XX:MaxHeapSize=2124414976
    C:\Users\SnailClimb>jinfo  -flag PrintGC 17340
    -XX:-PrintGC
    ```

    使用 jinfo 可以在不重启虚拟机的情况下，可以动态的修改 jvm 的参数。尤其在线上的环境特别有用,请看下面的例子：

    jinfo -flag [+|-]name vmid 开启或者关闭对应名称的参数。

    ```powershell
    C:\Users\SnailClimb>jinfo  -flag  PrintGC 17340
    -XX:-PrintGC

    C:\Users\SnailClimb>jinfo  -flag  +PrintGC 17340

    C:\Users\SnailClimb>jinfo  -flag  PrintGC 17340
    -XX:+PrintGC
    ```

### 生成堆转储快照
!!! tip "生成堆转储快照"

    jmap（Memory Map for Java）命令用于生成堆转储快照。 如果不使用 jmap 命令，要想获取 Java 堆转储，可以使用 “-XX:+HeapDumpOnOutOfMemoryError” 参数，可以让虚拟机在 OOM 异常出现之后自动生成 dump 文件，Linux 命令下可以通过 kill -3 发送进程退出信号也能拿到 dump 文件。

    jmap 的作用并不仅仅是为了获取 dump 文件，它还可以查询 finalizer 执行队列、Java 堆和永久代的详细信息，如空间使用率、当前使用的是哪种收集器等。和jinfo一样，jmap有不少功能在 Windows 平台下也是受限制的。

    示例：将指定应用程序的堆快照输出到桌面。后面，可以通过 jhat、Visual VM 等工具分析该堆文件。

    ```powershell
    C:\Users\SnailClimb>jmap -dump:format=b,file=C:\Users\SnailClimb\Desktop\heap.hprof 17340
    Dumping heap to C:\Users\SnailClimb\Desktop\heap.hprof ...
    Heap dump file created
    ```

### 分析 heapdump 文件
!!! tip "分析 heapdump 文件"

    jhat 用于分析 heapdump 文件，它会建立一个 HTTP/HTML 服务器，让用户可以在浏览器上查看分析结果。

    ```powershell
    C:\Users\SnailClimb>jhat C:\Users\SnailClimb\Desktop\heap.hprof
    Reading from C:\Users\SnailClimb\Desktop\heap.hprof...
    Dump file created Sat May 04 12:30:31 CST 2019
    Snapshot read, resolving...
    Resolving 131419 objects...
    Chasing references, expect 26 dots..........................
    Eliminating duplicate references..........................
    Snapshot resolved.
    Started HTTP server on port 7000
    Server is ready.
    ```

### 生成虚拟机当前时刻的线程快照
!!! tip "生成虚拟机当前时刻的线程快照"

    jstack（Stack Trace for Java）命令用于生成虚拟机当前时刻的线程快照。线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合.

    生成线程快照的目的主要是定位线程长时间出现停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等都是导致线程长时间停顿的原因。线程出现停顿的时候通过jstack来查看各个线程的调用堆栈，就可以知道没有响应的线程到底在后台做些什么事情，或者在等待些什么资源。

    ```powershell
    C:\Users\SnailClimb>jps
    13792 KotlinCompileDaemon
    7360 NettyClient2
    17396
    7972 Launcher
    8932 Launcher
    9256 DeadLockDemo
    10764 Jps
    17340 NettyServer

    C:\Users\SnailClimb>jstack 9256

    ```

### JVM线上问题排查和性能调优案例
!!! tip "JVM线上问题排查和性能调优案例"

    https://javaguide.cn/java/jvm/jvm-in-action.html