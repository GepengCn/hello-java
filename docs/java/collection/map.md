# Map

## 知识点

- HashMap
- ConcurrentHashMap
- LinkedHashMap
- HashTable
- TreeMap
- 设计模式

!!! info "HashMap"

    - JDK8之前是由数组+链表组成的，数组是HashMap主体，链表是为了解决哈希冲突，使用拉链法解决冲突
    - JDK8之后是由数组+链表+红黑树组成的，当链表长度大于阈值（默认为8），链表转化为红黑树，减少搜索时间
    - 拉链法的原理
    - 存储结构
    - put操作
    - 怎么确定捅下标
    - 计算hash值
    - 扩容基本原理
    - 计算数组容量
    - 链表转红黑树
    - 与HashTable比较

!!! info  "LinkedHashMap"

    继承自HashMap，增加了一条双向链表，使得可以保持键值对的插入顺序。顺序为插入顺序或LRU顺序

    剑指 Offer II 031. 最近最少使用缓存

    https://leetcode.cn/problems/OrIXps/solution/zui-jin-zui-shao-shi-yong-huan-cun-by-le-p3c2/

!!! info "HashTable"

    线程安全的

!!! info "TreeMap"

    基于红黑树实现

!!! info "ConcurrentHashMap"

    - 存储结构
    - size操作
    - jdk1.8的改动
