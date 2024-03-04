# Map

## 思维导图

![](https://p.ipic.vip/e25rdn.jpg)

## 问题

- 介绍下HashMap
- 什么是扰动函数,怎么实现的
- jdk8前后有什么变化，为什么？
- 为什么需要链表
- 什么是hash冲突
- 为什么要用红黑树？红黑树是什么？红黑树需要满足什么条件？有什么优势？与平衡二叉树的区别？使用场景？
- 为什么改为尾插法，头插法有什么弊端
- 链表和红黑树转换的阈值是多少，为什么不一样
- HashMap怎么扩容
- HashMap为什么重写equals之后，必须重写hashcode
- HashMap怎么保证线程安全
- HashMap是否有序，如果想要有序，有什么方案？
- LinkedHashMap怎么实现有序，有哪些顺序，LRU顺序是什么？算法原理是什么？
- 描述下put操作
- 怎么确定捅下标
- 与hashtable比较
- 与hashset比较
- treemap描述下？
- ConcurrentHashMap描述下？jdk8前后有什么变化？怎么保证的线程安全？
- hashmap的长度为什么是2的幂次方
- hashmap的遍历方式
- hashmap中用了哪些设计模式




## 答案

!!! tip "介绍下HashMap"

    - jdk8之前:数组+链表
    - jdk8之后:数组+链表+红黑树
    - 增删查的时间复杂度为O(1)
    - HashMap 通过 key 的 hashcode 经过扰动函数处理过后得到 hash 值，然后通过 (n - 1) & hash 判断当前元素存放的位置（这里的 n 指的是数组的长度），判断链表中是否存在该key，存在则覆盖value，不存在，则尝试插入新元素，先判断是否超出数组容量扩容阈值，如果超出则先扩容，否则创建新链表并写入元素在元素存放的位置。
    
    
!!! tip "什么是扰动函数,怎么实现的"

    所谓扰动函数指的就是 HashMap 的 hash 方法。使用 hash 方法也就是扰动函数是为了防止一些实现比较差的 hashCode() 方法 换句话说使用扰动函数之后可以减少碰撞。
    
    ```java
    static final int hash(Object key) {
        int h;
        // key.hashCode()：返回散列值也就是hashcode
        // ^：按位异或
        // >>>:无符号右移，忽略符号位，空位都以0补齐
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
    ```

!!! tip "jdk8前后有什么变化，为什么？"
    
    1. 结构发生变化，jdk8之后引入了红黑树，为了解决hash冲突比较严重时候，链表数量较大时查询较慢的问题
    2. hash方法（扰动函数）简化与优化
    
    - [x] jdk8
    
    ```java
    static final int hash(Object key) {
        int h;
        // key.hashCode()：返回散列值也就是hashcode
        // ^：按位异或
        // >>>:无符号右移，忽略符号位，空位都以0补齐
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
    
    ```
    
    - [x] jdk7
    
    ```java
    static int hash(int h) {
        // This function ensures that hashCodes that differ only by
        // constant multiples at each bit position have a bounded
        // number of collisions (approximately 8 at default load factor).

        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
    }
    ```
    
    相比于 JDK1.8 的 hash 方法 ，JDK 1.7 的 hash 方法的性能会稍差一点点，因为毕竟扰动了 4 次。
    
    
!!! tip "为什么需要链表"
    
    解决hash冲突
    
!!! tip "什么是hash冲突"

    用哈希表存储数据时， 多个不同的键（Key）被哈希函数映射到同一个位置
    
!!! tip "为什么要用红黑树？红黑树是什么？红黑树需要满足什么条件？有什么优势？与平衡二叉树的区别？使用场景？"

    1. 红黑树增删查的时间复杂度是O(logn)，链表是O(n)，链表数量较大时，转换为红黑树，提升查询效率
    2. 红黑树是一个二叉搜索树，增加了一个存储位表示结点颜色，没有一个路径会比其它路径长2倍，所以是近乎平衡的
    3. 需要满足5个条件
        - 每个结点或是红色的，或是黑色的
        - 根结点是黑色的
        - 每个叶结点是黑色的
        - 如果一个结点是红色的，则它的两个子结点都是黑色的
        - 对每个结点，从该结点到其所有后代叶结点的简单路径上，均包含相同数目的黑色结点
    4. 两点优势
        - 红黑树是非AVL树，跟AVL树相比，在插入或删除元素时，不需要旋转很多次以保持树的平衡，效率大大提升。
        - 二叉查找树在一定情况下，会退化成一个线性结构，红黑树则不会出现这种情况
    5. TreeMap、TreeSet以及JDK1.8之后的HashMap底层都用到了红黑树
    
!!! tip "为什么改为尾插法，头插法有什么弊端"

    头插法：新值会取代原值，原值插入链表，头插法会使链表发生反转，多线程环境下会产生环
    尾插法：遍历链表，在尾部插入新值
    
!!! tip "链表和红黑树转换的阈值是多少，为什么不一样"

    - 链表长度大于8&数组大小>=64，链表->红黑树
    - 链表长度小于6，红黑树->链表
    
    -   为什么是8？为什么是6？hash 函数设计合理的情况下，发生 hash 碰撞 8 次的几率为百万分之 6，8够用了；至于为什么转回来是 6，因为如果 hash 碰撞次数在 8 附近徘徊，会一直发生链表和红黑树的转化，为了预防这种情况的发生。
    
!!! tip "HashMap怎么扩容"

    - 创建一个新数组，长度是2倍
    - rehash，遍历原数组，把所有的节点重新hash到新数组
    - 为什么要重新hash，不能直接复制过去吗？因为hash规则中，长度变了，hash值也随之变了
    
!!! tip "HashMap为什么重写equals之后，必须重写hashcode"
    
    因为hashmap哈希冲突时，会有多个值存储在同一个链表中，当从链表中获取key时，需要用equals来查找目标值。如果不重写，就会出现用目标值作为key查不到对应的值的情况
    
!!! tip "HashMap怎么保证线程安全"

    1. Collections.synchronizedMap()
    2. 使用synchronized关键字
    3. 使用HashTable替换
    4. 最好是使用ConcurrentHashMap来替换
    
!!! tip "HashMap是否有序，如果想要有序，有什么方案？"

    无序，使用LinkedHashMap或TreeMap
    
!!! tip "LinkedHashMap怎么实现有序，有哪些顺序，LRU顺序是什么？算法原理是什么？"

    1. 内部维护了一个双向链表，用来维护顺序或者LRU排序
    2. 插入顺序或LRU顺序
    3. LRU是Least Recently Used的缩写，即最近最少使用，它能保证最新使用的数据排在前面，一种常用的缓存算法
    4. LRU 缓存机制可以通过哈希表辅以双向链表实现，我们用一个哈希表和一个双向链表维护所有在缓存中的键值对。
        - 双向链表按照被使用的顺序存储了这些键值对，靠近头部的键值对是最近使用的，而靠近尾部的键值对是最久未使用的。
        - 哈希表即为普通的哈希映射（HashMap），通过缓存数据的键映射到其在双向链表中的位置。
        - 参考https://leetcode.cn/problems/OrIXps/solution/zui-jin-zui-shao-shi-yong-huan-cun-by-le-p3c2/
        
!!! tip "描述下put操作"
    
    1. 先通过hash方法确定元素下标
    2. 判断该元素位置链表是否为空，如果不为空并且key已存在，则直接更新value值
    3. 否则尾插法插入新键值对到链表中

!!! tip "怎么确定捅下标"

    hash & (n-1)
    
    hash值hash方法或扰动函数
    
    n代表哈希表最大长度
    
!!! tip "与hashtable比较"

    1. hashtable是线程安全的，hashmap不是
    2. hashmap效率高于hashtable
    3. hashmap可存储null的key和value，key只能有一个。hashtable不可，存储会抛异常
    4. hashmap初始容量为16，之后每次扩容，容量是之前的2倍。hashtable初始容量为11，之后每次扩容为之前的2n+1。如果创建时指定容量大小，hashtable会使用指定大小，而hashmap会扩容为2的幂次方
    5. hashmap增加了链表和红黑树解决hash冲突问题，hashtable没有这样的机制
    
!!! tip "与hashset比较"

    1. hashmap存储键值对，hashset仅存储对象
    2. hashset底层是hashmap
    
!!! tip "treemap描述下？"

    TreeMap是一个有序的哈希表，底层是由红黑树实现的，天然支持排序
    
!!! tip "ConcurrentHashMap描述下？jdk8前后有什么变化？怎么保证的线程安全？"

    ConcurrentHashMap 底层是基于 数组 + 链表 组成的，不过在 jdk7 和 jdk8 中具体实现稍有不同
    
    jdk7中对整个桶数组进行了分段分割，每段都加了显式锁，多线程访问桶里不同段的数据，就不会存在锁竞争，并发数为分段个数
    
    jdk8中用数组+链表/红黑树的数据结构来实现，并发主要使用synchronized和CAS操作来控制，synchronized用来锁定链表的头节点，锁的粒度更细，效率更高；CAS主要用在查找、赋值操作；扩容时，阻塞所有读写操作，但是可并发扩容
    
!!! tip "hashmap的长度为什么是2的幂次方"

    1. 计算方便，简化取模操作，位运算更快
    2. 分布均匀，n-1的2进制都是11..111的形式，在与添加的元素的hash值进行位运算时能够充分的散列，使添加的元素能均匀的分布减少hash碰撞
    
!!! tip "hashmap的遍历方式"

    HashMap 遍历从大的方向来说，可分为以下 4 类：

    1. 迭代器（Iterator）方式遍历；
    2. For Each 方式遍历；
    3. Lambda 表达式遍历（JDK 1.8+）;
    4. Streams API 遍历（JDK 1.8+）。

    但每种类型下又有不同的实现方式，因此具体的遍历方式又可以分为以下 7 种：

    1. 使用迭代器（Iterator）EntrySet 的方式进行遍历；
    2. 使用迭代器（Iterator）KeySet 的方式进行遍历；
    3. 使用 For Each EntrySet 的方式进行遍历；
    4. 使用 For Each KeySet 的方式进行遍历；
    5. 使用 Lambda 表达式的方式进行遍历；
    6. 使用 Streams API 单线程的方式进行遍历；
    7. 使用 Streams API 多线程的方式进行遍历。
    
    https://mp.weixin.qq.com/s/zQBN3UvJDhRTKP6SzcZFKw
    
!!! tip "hashmap中用了哪些设计模式"

    迭代器模式