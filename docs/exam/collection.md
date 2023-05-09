# 集合

## 思维导图

![](https://p.ipic.vip/3no7g4.jpg)

## 问题
1. List, Set, Queue, Map 四者的区别
2. List底层数据结构
3. Set底层数据结构
4. Queue底层数据结构
5. ArrayList 与 LinkedList 区别
6. HashSet、LinkedHashSet 和 TreeSet 三者的异同
7. Queue 与 Deque 的区别
8. ArrayDeque 与 LinkedList 的区别
9. 说一说 PriorityQueue



## 答案

!!! tip "List, Set, Queue, Map 四者的区别"

    - List: 存储的元素是有序的，可重复的
    - Set: 存储的元素是有序的，不可重复的
    - Queue: 存储的元素是有序的、可重复的
    - Map: key是无序的、不可重复的，value是无序的、可重复的



!!! tip "List底层数据结构"

    - ArrayList: 数组
    - Vector: 数组
    - LinkedList: 双向链表

!!! tip "Set底层数据结构"

    - HashSet: 无序，唯一，基于HashMap
    - LinkedHashSet: 有序，HashSet的子类，内部是`LinkedHashMap`实现的
    - TreeSet:有序，唯一，红黑树

!!! tip "Queue底层数据结构"

    - PriorityQueue: 数组实现的二叉堆
    - ArrayQueue: 数组+双指针

!!! tip "ArrayList 与 LinkedList 区别"

    1. 底层数据结构：ArrayList底层是数组，LinkedList底层是双向链表
    2. 查询效率：ArrayList为数组，所以是O(1)；LinkedList是链表，则为O(N)。
    3. 插入与删除是否受元素位置影响：
        ArrayList: 如果是插入或者删除末尾元素，时间复杂度为O(1)；如果是指定位置i插入或者删除，i之后的元素需要向后或者向前移动，则时间复杂度为O(N)
        LinkedList: 如果是插入或者删除头部或者末尾元素，时间复杂的位O(1)；如果是指定位置i插入或者删除，需要先查找到指定位置，所以时间复杂度为O(N)
    4. 内存空间占用：ArrayList是数组，所以会预留一部分容量空间；LinkedList是双向链表，需要有额外的指针指向前后元素。

!!! tip "HashSet、LinkedHashSet 和 TreeSet 三者的异同"

    - HashSet、LinkedHashSet 和 TreeSet 都是 Set 接口的实现类，都能保证元素唯一，并且都不是线程安全的。
    - HashSet、LinkedHashSet 和 TreeSet 的主要区别在于底层数据结构不同。HashSet 的底层数据结构是哈希表（基于 HashMap 实现）。LinkedHashSet 的底层数据结构是链表和哈希表，元素的插入和取出顺序满足 FIFO。TreeSet 底层数据结构是红黑树，元素是有序的，排序的方式有自然排序和定制排序。
    - 底层数据结构不同又导致这三者的应用场景不同。HashSet 用于不需要保证元素插入和取出顺序的场景，LinkedHashSet 用于保证元素的插入和取出顺序满足 FIFO 的场景，TreeSet 用于支持对元素自定义排序规则的场景。#


!!! tip "Queue 与 Deque 的区别"

    - Queue 是单端队列，只能从一端插入元素，另一端删除元素，实现上一般遵循 先进先出（FIFO） 规则。
    - Deque 是双端队列，在队列的两端均可以插入或删除元素。

!!! tip "ArrayDeque 与 LinkedList 的区别"

    ArrayDeque 和 LinkedList 都实现了 Deque 接口，两者都具有队列的功能

    - ArrayDeque 是基于可变长的数组和双指针来实现，而 LinkedList 则通过链表来实现。
    - ArrayDeque 不支持存储 NULL 数据，但 LinkedList 支持。
    - ArrayDeque 是在 JDK1.6 才被引入的，而LinkedList 早在 JDK1.2 时就已经存在。
    - ArrayDeque 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)。虽然 LinkedList 不需要扩容，但是每次插入数据时均需要申请新的堆空间，均摊性能相比更慢。

!!! tip "说一说 PriorityQueue"

    - PriorityQueue 利用了二叉堆的数据结构来实现的，底层使用可变长的数组来存储数据
    - PriorityQueue 通过堆元素的上浮和下沉，实现了在 O(logn) 的时间复杂度内插入元素和删除堆顶元素。
    - PriorityQueue 是非线程安全的，且不支持存储 NULL 和 non-comparable 的对象。
    - PriorityQueue 默认是小顶堆，但可以接收一个 Comparator 作为构造参数，从而来自定义元素优先级的先后。