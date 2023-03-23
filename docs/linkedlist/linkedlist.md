# 链表`linkedlist`

## 特性
- 表示一列元素
- 递归的数据结构
- 线性
- 内存非连续的
- 每个节点含有一个泛型的元素和一个指向另一条链表的引用
- 插入和删除效率很高，只需要改变节点的指向
- 查询效率低，时间复杂度 $O(N)$
- 与数组相比，内存占用高，因为需要保存指针

## 构造链表

```java
public class ListNode{
    int val;
    ListNode next;
}
```
!!! Note "初始化方法"

    1. 初始化各个节点对象
    2. 构建节点的指向关系

## 初始化
!!! Info "构造器初始化"

    ```java
    ListNode node = new ListNode(1);//构造器初始化
    ```

!!! Info "创建对象后初始化"

    ```java
    ListNode node1 = new ListNode();
    node1.val = 2;
    ```

## 常用操作

!!! Quote "遍历"

    ```java
    ListNode head = initNode();
    while(head!=null){
        System.out.println(head.val);
        head = head.next;
    }

    ```

!!! Quote "查找"

    ```java
    public ListNode find(ListNode head, int target){
        while(head!=null){
            if(head.val == target)return head;
            head = head.next;
        }
        return null;
    }
    ```
!!! Quote "插入"

    ```java
    public void insert(ListNode head, int newValue){
        ListNode newNode = new ListNode(newValue);
        head.next = newNode;
    }
    ```

!!! Quote "删除"

    ```java
    public void delete(ListNode head, int target){
        ListNode pre = null;
        while(head!=null){
            if(target == head.val){
                if(pre==null){
                    head = head.next;
                } else {
                    pre.next = head.next;
                }
            }else {
                pre = head;
                head = head.next;
            }
        }
    }
    ```

## 常见类型

- **单向链表**。节点指向下一节点
- **环形链表**。首位节点相接，即尾结点指向首节点，形成环。
- **双向链表**。节点指向上一节点与下一节点。

## 实际应用

!!! Note "缓存算法"

!!! Note "浏览器历史记录"

## `leetcode`

### 146. LRU 缓存
!!! example "146. LRU 缓存"

    请你设计并实现一个满足  LRU (最近最少使用) 缓存 约束的数据结构。
    实现 LRUCache 类：

    - LRUCache(int capacity) 以 正整数 作为容量 capacity 初始化 LRU 缓存
    - int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
    - void put(int key, int value) 如果关键字 key 已经存在，则变更其数据值 value ；如果不存在，则向缓存中插入该组 key-value 。如果插入操作导致关键字数量超过 capacity ，则应该 逐出 最久未使用的关键字。
    - 函数 get 和 put 必须以 $O(1)$ 的平均时间复杂度运行。

    ```java
    public class LRUCache {
        HashMap<Integer, Node> map;
        DoubleLinkedList cache;
        int cap;
        public LRUCache(int capacity){
            map   = new HashMap<>();
            cache = new DoubleLinkedList();
            cap   = capacity;
        }

        public void put(int key, int val){
            Node newNode = new Node(key, val);

            if(map.containsKey(key)){
                cache.delete(map.get(key));
                cache.addFirst(newNode);
                map.put(key, newNode);
            }else{
                if(map.size() == cap){
                    int k = cache.deleteLast();
                    map.remove(k);
                }
                cache.addFirst(newNode);
                map.put(key, newNode);

            }
        }

        public int get(int key){
            if(!map.containsKey(key))   return -1;

            int val = map.get(key).val;
            put(key, val);

            return val;
        }
    }

    /**
    *  head: recently used
    *  tail: LRU
    */
    class DoubleLinkedList{
        Node head;
        Node tail;

        public DoubleLinkedList(){
            head = new Node(0,0);
            tail = new Node(0,0);

            head.next = tail;
            tail.prev = head;
        }

        public void addFirst(Node node){

            node.next   = head.next;
            node.prev   = head;

            head.next.prev = node;
            head.next      = node;
        }

        public int delete(Node n){
            int key = n.key;
            n.next.prev = n.prev;
            n.prev.next = n.next;

            return key;
        }

        public int deleteLast(){
            if(head.next == tail)   return -1;

            return delete(tail.prev);
        }
    }

    class Node{
        public int key;
        public int val;
        public Node prev;
        public Node next;

        public Node(int key, int val){
            this.key = key;
            this.val = val;
        }
    }

    ```
### 460. LFU 缓存
!!! example "460. LFU 缓存"

    请你为 最不经常使用（LFU）缓存算法设计并实现数据结构。

    实现 LFUCache 类：

    - LFUCache(int capacity) - 用数据结构的容量 capacity 初始化对象
    - int get(int key) - 如果键 key 存在于缓存中，则获取键的值，否则返回 -1 。
    - void put(int key, int value) - 如果键 key 已存在，则变更其值；如果键不存在，请插入键值对。当缓存达到其容量 capacity 时，则应该在插入新项之前，移除最不经常使用的项。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，应该去除 最近最久未使用 的键。
    - 为了确定最不常使用的键，可以为缓存中的每个键维护一个 使用计数器 。使用计数最小的键是最久未使用的键。

    当一个键首次插入到缓存中时，它的使用计数器被设置为 1 (由于 put 操作)。对缓存中的键执行 get 或 put 操作，使用计数器的值将会递增。

    函数 get 和 put 必须以 $O(1)$ 的平均时间复杂度运行。



    ```java
    class LFUCache {
        Map<Integer, Node> cache; // 存储缓存的内容
        Map<Integer, DoublyLinkedList> freqMap; // 存储每个频次对应的双向链表
        int size;
        int capacity;
        int min; // 存储当前最小频次

        public LFUCache(int capacity) {
            cache = new HashMap<> (capacity);
            freqMap = new HashMap<>();
            this.capacity = capacity;
        }

        public int get(int key) {
            Node node = cache.get(key);
            if (node == null) {
                return -1;
            }
            freqInc(node);
            return node.value;
        }

        public void put(int key, int value) {
            if (capacity == 0) {
                return;
            }
            Node node = cache.get(key);
            if (node != null) {
                node.value = value;
                freqInc(node);
            } else {
                if (size == capacity) {
                    DoublyLinkedList minFreqLinkedList = freqMap.get(min);
                    cache.remove(minFreqLinkedList.tail.pre.key);
                    minFreqLinkedList.removeNode(minFreqLinkedList.tail.pre); // 这里不需要维护min, 因为下面add了newNode后min肯定是1.
                    size--;
                }
                Node newNode = new Node(key, value);
                cache.put(key, newNode);
                DoublyLinkedList linkedList = freqMap.get(1);
                if (linkedList == null) {
                    linkedList = new DoublyLinkedList();
                    freqMap.put(1, linkedList);
                }
                linkedList.addNode(newNode);
                size++;
                min = 1;
            }
        }

        void freqInc(Node node) {
            // 从原freq对应的链表里移除, 并更新min
            int freq = node.freq;
            DoublyLinkedList linkedList = freqMap.get(freq);
            linkedList.removeNode(node);
            if (freq == min && linkedList.head.post == linkedList.tail) {
                min = freq + 1;
            }
            // 加入新freq对应的链表
            node.freq++;
            linkedList = freqMap.get(freq + 1);
            if (linkedList == null) {
                linkedList = new DoublyLinkedList();
                freqMap.put(freq + 1, linkedList);
            }
            linkedList.addNode(node);
        }
    }

    class Node {
        int key;
        int value;
        int freq = 1;
        Node pre;
        Node post;

        public Node() {}

        public Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    class DoublyLinkedList {
        Node head;
        Node tail;

        public DoublyLinkedList() {
            head = new Node();
            tail = new Node();
            head.post = tail;
            tail.pre = head;
        }

        void removeNode(Node node) {
            node.pre.post = node.post;
            node.post.pre = node.pre;
        }

        void addNode(Node node) {
            node.post = head.post;
            head.post.pre = node;
            head.post = node;
            node.pre = head;
        }
    }

    ```
### 1472. 设计浏览器历史记录
!!! example "1472. 设计浏览器历史记录"

    你有一个只支持单个标签页的 浏览器 ，最开始你浏览的网页是 `homepage` ，你可以访问其他的网站 `url` ，也可以在浏览历史中后退 `steps` 步或前进 `steps` 步。

    请你实现 `BrowserHistory` 类：

    - `BrowserHistory(string homepage)` ，用 `homepage` 初始化浏览器类。
    - `void visit(string url)` 从当前页跳转访问 `url` 对应的页面  。执行此操作会把浏览历史前进的记录全部删除。
    - `string back(int steps)` 在浏览历史中后退 `steps` 步。如果你只能在浏览历史中后退至多 `x` 步且 `steps > x` ，那么你只后退 `x` 步。请返回后退 至多 `steps` 步以后的 `url` 。
    - `string forward(int steps)` 在浏览历史中前进 `steps` 步。如果你只能在浏览历史中前进至多 `x` 步且 `steps > x` ，那么你只前进 `x` 步。请返回前进 至多 `steps`步以后的 `url` 。


    ```java
    class BrowserHistory {
        Node page;
        Node temp = page;
        public BrowserHistory(String homepage) {
            page = new Node(homepage);
        }

        public void visit(String url) {
            Node newPage = new Node(url);
            newPage.next = null;
            page.next = newPage;
            newPage.pre = page;
            page = page.next;
        }

        public String back(int steps) {
            while (steps != 0) {
                if (page.pre == temp) {
                    break;
                }else {
                    page = page.pre;
                    steps--;
                }
            }
            return page.str;
        }

        public String forward(int steps) {
            while (steps != 0) {
                if (page.next == null) {
                    break;
                }else {
                    page = page.next;
                    steps--;
                }
            }
            return page.str;
        }
    }

    class Node {
        String str;
        Node pre;
        Node next;
        Node(String str) {this.str = str;}
    }
    ```