 - 介绍下HashMap
 
 一个哈希表，jdk8之前是由数组和链表组成的，jdk8之后是由数组和链表/红黑树组成的
 
 由于存储结构主体是数组，它的增删改效率很高，时间复杂度是O(1)
 
 链表是用来解决hash冲突
 
 hash表的数组下标是通过对key值的hash与数组的长度做位运算求出来的
 
 但是极端情况下，还是会出现hash冲突，就是求出同一个下标，这时候需要将该值，插入链表尾部，以此解决hash冲突
 
 如果需要查询的时候，同样先求出数组下标，然后通过equals方法，从链表中遍历该key值
 
 当链表的长度大于8，并且数组的长度大于64的之后，链表会转化为红黑树
 
 因为链表的检索时间复杂度为O(n)，当过长的时候，效率变差，使用红黑树，会将时间复杂度降为O(logn)提升性能
 
 
 - 什么是扰动函数,怎么实现的
 
 就是hash函数
 
 - jdk8前后有什么变化，为什么？
 
 
 
 - 为什么需要链表
 - 什么是hash冲突
 - 为什么要用红黑树？红黑树是什么？红黑树需要满足什么条件？有什么优势？与平衡二叉树的区别？使用场景？
 
 红黑树是一种二叉搜索树
 
 1. 根节点是黑色
 2. 叶节点是黑色
 3. 叶结点到根节点的红结点一样的
 4. 红结点下面是黑结点
 5. 每个结点或是红或是黑
 
 1. 与avl树相比，插入或者删除不用旋转很多次来保持平衡，效率大大提升
 2. 二叉查找树极端情况下会退化为线性结构，红黑树则不会
 
 TreeMap，hashMap treeset都用到了红黑树
 
 - 为什么改为尾插法，头插法有什么弊端
 
 头插法在并发情况下会形成环
 
 - 链表和红黑树转换的阈值是多少，为什么不一样
 
 链表->红黑树 阈值8&&数组长度>64
 
 红黑树->链表 阈值6
 
 如果在同样的阈值周围hash碰撞，会一直发生两者的转换，为了防止这种情况的发生
 
 - HashMap怎么扩容
 
 1. 构建新数组，长度为之前的2倍
 
 2. 将旧数组的值 重新hash到新数组当中
 
 
 - HashMap为什么重写equals之后，必须重写hashcode
 
    默认用对象的内存地址hash，如果不重写hashcode在hash表中，会认为是不等的两个对象
 - HashMap怎么保证线程安全
 
 Collections.synchronizedMap
 
 HashTable
 
 ConcurrentMap
 
 - HashMap是否有序，如果想要有序，有什么方案？
 否
 LinkedHashMap
 TreeMap
 
 - LinkedHashMap怎么实现有序，有哪些顺序，LRU顺序是什么？算法原理是什么？
 
 Entry结点增加了个双向链表，缓存了前驱和后继。插入顺序和LRU顺序，LRU，最近最少使用，最近使用的会排在最前面，使用的就是双链表结构，插入从头部插入，获取也从头部获取
 
 - 描述下put操作
 
 先通过hash确定元素下标
 判断该位置的元素是否为空，如果不为空，遍历链表，通过equals方法确定key是否存在链表，如果存在，直接更新value，不存在，尾插法插入链表尾部
 
 
 
 
 
 - 怎么确定捅下标
 
 hash&(n-1)
 
 - 与hashtable比较
 
 hashtable线程安全，hashmap非线程安全
 
 性能hashmap更好
 
 hashmap key和value都可以存入null，key只能存一个，hashtable会报错
 
 hashmap每次扩容，都是之前的2倍，初始化16，如果指定初始化容量，一定会扩容到2幂次方
 hashtable每次扩容，都是之前的2n+1，初始化11，如果指定多少，就是多少
 
 - 与hashset比较
 
 hashmap键值对
 
 hashset就是值
 
 - treemap描述下？
 
 内部是由红黑树构成
 
 - ConcurrentHashMap描述下？jdk8前后有什么变化？怎么保证的线程安全？
 
 jdk8之前是分段数组+链表构成
 
 内部类Segment，保存了一组数组，共用一个锁，多线程访问不同Segment的数据，不存在锁竞争，可以并发执行，并发最大数就是segment的数量
 
 jdk8之后是数组+链表/红黑树
 
 使用synchronized关键字和CAS操作来保证线程安全，synchronized锁定锁定链表的头部，锁粒度更细，效率更高，CAS用来查找和赋值操作，扩容时会阻塞读写操作，但是可以并发扩容
 
 - hashmap的长度为什么是2的幂次方
 
 hash&(n-1) 相比于hash%n 位运算效率更高
 
 如果n是2的幂次方， hash&(n-1) 会hash更充分，更均匀，hash碰撞的频率会更低
 
 - hashmap的遍历方式
 
 Iterator模式
 
 EntrySet
 
 KeySet
 
 ForEach
 
 Lambda
 
 Streams API
 
 - hashmap中用了哪些设计模式