- 运行时数据区域概述

jdk8之前

线程共享的：堆、方法区、运行时常量池
线程私有的：虚拟机栈、本地方法栈、程序计数器
本地内存：直接内存

jdk8之后

线程共享的：堆
线程私有的：虚拟机栈、本地方法栈、程序计数器
本地内存：直接内存、元空间（方法区、运行时常量池）

- 程序计数器

线程私有，线程通过改变程序计数器的值，来获取下一步要执行的指令，分支、循环、异常、跳转、线程恢复等到时靠计数器实现的。另外，为了让线程恢复到切换之前的正确位置，也需要程序计数器，而且线程之间计数器是独立的，单独存储的

- Java 虚拟机栈

线程私有，生命周期与线程一样，随着线程的创建而创建，随着线程的死亡而死亡。

所有java方法的调用都是依赖虚拟机栈来完成的，每次方法的调用都有一个栈帧插入栈，方法调用完毕栈帧弹出。

虚拟机栈是由栈帧组成的，栈帧包含动态链接、返回地址、操作数栈、局部变量表

局部变量表：编译时确定的各种的对象的数据类型和对象引用
操作数栈：存放方法执行过程中的中间计算机结果和临时变量
动态链接：将符号引用转换为直接引用
返回地址：一种是return返回的，一种是异常，都伴随着栈帧的弹出

- 程序运行中栈可能会出现的错误


StackOverFlowError:如果虚拟机栈不可以动态扩展，如果方法调用的深度超过虚拟机栈的最大深度，抛出这个异常
OutOfMemoryError:如果虚拟机栈可以动态扩展，但是无法申请到足够的内存，则抛出这个异常

- 本地方法栈

与虚拟机类似，所有Native方法调用都是通过本地方法栈完成的

- 堆

jvm中管理内存最大的一块区域，主要用来存储对象实例

也是垃圾回收器主要回收的区域，也叫GC堆

- 堆可能出现的错误

OutOfMemoryError: GC Overhead Limit Exceeded 当jvm花太多时间执行垃圾回收但能回收很小的堆空间时
OutOfMemoryError: Java heap space 创建对象时，堆剩余空间不足以存放的时候

- 方法区

用来存放类的信息、字段信息、变量信息、方法信息等

- 方法区和永久代以及元空间是什么关系呢？

类似于接口和实现类

方法区就是接口，永久代和元空间分别是方法区的实现

永久代是jdk8之前的
元空间是jdk8之后的

- 为什么要将永久代替换为元空间呢?

永久代的内存大小是固定的，无法动态扩展，云空间挪到了本地内存当中，内存可以动态扩展，发生内存溢出的几率要比之前小很多


- 运行时常量池

存储各种符号和常量的池

- 字符串常量池

单独开辟的一块区域，用来存储字符串，为了提高字符串创建与访问的效率而设

- JDK 1.7 为什么要将字符串常量池移动到堆中？

为了更高效的gc回收，挪到堆中，能够被gc更快更轻量的进行回收

- 直接内存

不是jvm直接管理的内存，而是通过jni调用Native方法管理的

nio中通过使用Native方法直接创建堆外内存，然后通过DirectByteBuffer来管理这个部分内存，这个在某些场景下能大幅提升效率，因为它避免了在堆外和堆内来回搬运数据

- Java 对象的创建过程

加载
验证

初始化
使用
销毁
- 内存分配的两种方式

内存碰撞

空闲列表

- 内存分配并发问题

cas操作

- 对象的内存布局



- 对象的访问定位



- 内存分配和回收原则


- 空间分配担保

判断老年代的内存可否容纳新生代的内存，如果可以则minorGC，否则FullGC

- 死亡对象判断方法

引用计数法
可达性分析算法

- 哪些对象可以作为 GC Roots 呢？

1. 虚拟机栈引用的对象
2. 本地方法栈引用的对象
3. 常量
4. 静态变量
5. 同步锁引用的变量

- 对象可以被回收，就代表一定会被回收吗？

会经过两次标记，第一次可达性分析算法标记之后，还存活的，会存入一个队列中，然后等待第二次标记之后，这个对象依然没有任何引用，才会真正被回收

- 引用类型有哪些

强引用
软引用
弱引用
虚引用


- 如何判断一个类是无用的类？

- 所有的对象实例都被回收
- 类的ClassLoader都被回收
- 类的Class没有在任何地方被引用，也不能通过反射访问类的方法

- 垃圾收集算法有哪些？

标记、清除

标记、整理

复制

分代

- 垃圾收集器有哪些？

Serial

Serial Old

ParNew

Parallel Old

G1

CMS

ZGC


- 类加载器介绍

BootstrapClassLoader
ExtensionClassLoader
AppClassLoader
CustomCLassLoader


- 双亲委派模型介绍

就是子加载器加载对象之前，先交给父级去加载，只有父级加载不了的情况下，才会尝试自己加载

- 双亲委派模型的执行流程

1. 先判断是否已经被加载，如果已加载，直接返回
2. 判断父级能否加载，如果父级可以，则交给父级加载
3. 如果父级不能，则尝试自己加载

- 双亲委派模型的好处

提高稳定性，避免类重复，避免创建同名类，影响基础类

- 打破双亲委派模型方法

重写loadClass

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