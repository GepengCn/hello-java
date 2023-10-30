- 介绍下HashMap
1.8之前是数组+链表
1.8之后是数组+链表+红黑树
链表是为了解决哈希冲突
红黑树是为了解决哈希冲突严重，链表长度较大，查询性能慢的问题

- 什么是扰动函数,怎么实现的

hash方法，内部就是位运算

- jdk8前后有什么变化，为什么？

1.8之后使用红黑树优化哈希冲突严重，链表过长引起的查询慢问题

- 为什么需要链表

解决hash冲突

- 什么是hash冲突

就是通过hash&(n-1)获取的数组下标出现重复

- 为什么要用红黑树？红黑树是什么？红黑树需要满足什么条件？有什么优势？与平衡二叉树的区别？使用场景？

红黑树新增和查询都是O(logn)，链表是O(n)。

红黑树是一种特殊的二叉查找树，里面增加了一个标志位记录颜色。

1. 红色或黑色
2. 根节点黑色
3. 叶节点黑色
4. 红结点下面一定是黑色
5. 每个叶节点的黑高一样

优势：
相比如AVL树，不必每次插入或删除数据都旋转
二叉查找树极端情况下会退化成一个线性结构，红黑树不会

TreeMap和HashMap中都用到了

- 为什么改为尾插法，头插法有什么弊端

避免并发情况下，头插法可能会出现环

- 链表和红黑树转换的阈值是多少，为什么不一样

链->红黑树 阈值8&&数组长度大于64
红黑树->链 6
避免哈希冲突发生在阈值附近，导致来回的转换

- HashMap怎么扩容

创建一个新数组
将原数组的元素，遍历，分别重新hash到新数组上

- HashMap为什么重写equals之后，必须重写hashcode

hashmap获取数组下标用到了hashcode，如果使用一个对象作为key，不重写，通过hashcode每次都不一样

- HashMap怎么保证线程安全

Collections.synchronizedMap
HashTable
ConcurrentHashMap

- HashMap是否有序，如果想要有序，有什么方案？
无序
LinkedHashMap


- LinkedHashMap怎么实现有序，有哪些顺序，LRU顺序是什么？算法原理是什么？

内部维护了一个双向链表
顺序和LRU
LRU是最新最少使用


- 描述下put操作

1. 先判断是否需要扩容，需要则扩容
2. 通过hash&(n-1)获取数组下标，如果当前位置没有元素，则尾插法写入元素
3. 如果当前位置有元素，则遍历链表，判断当前key是否存在，如果存在，更新value，如果不存在，尾插法写入链表

- 怎么确定捅下标

hash&(n-1)

- 与hashtable比较

hashmap现成不安全
hashtable线程安全
hashmap性能更好
hashmap key和value可以为null
hashmap通过链表和红黑树解决hash冲突，hashtable没有这个机制

- 与hashset比较

hashset内部就是一个hashmap
hashmap存储key和value
hashset只存储key

- treemap描述下？

内部是由红黑树构成的，时间复杂度为O(logn)，支持排序

- ConcurrentHashMap描述下？jdk8前后有什么变化？怎么保证的线程安全？

1.8之前是数组+链表，由一个内部类Segment构成，Segment是一个分段锁，每一个Segment管理一组数组元素，当并发请求的元素不在同一个Segment中时，就不会出现并发冲突
1.8之后是数组+链表+红黑树，使用synchronized和CAS来控制并发，synchronized锁住链表头部，锁的粒度更细，CAS来控制查询和修改元素。

- hashmap的长度为什么是2的幂次方

1. 可以用位运算取代求余，性能更好
2. 让hash更均匀

- hashmap的遍历方式

迭代器模式
EntrySet Itertor
KeySet Itertor

ForEach EntrySet
ForEach KeySet

Lambda

Stream API 单线程
Stream API 多线程




- hashmap中用了哪些设计模式

迭代器模式