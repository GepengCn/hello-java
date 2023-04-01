# 堆`heap`

## 思维导图

![](https://p.ipic.vip/33ee5n.png#only-light)
![](https://p.ipic.vip/om2mko.png#only-dark)

## 定义

分为两种类型，最大堆（大顶堆）和最小堆（小顶堆）：

1. 最大堆：任意结点$>=$其子结点
2. 最小堆：任意结点$<=$其子结点

## 特性

- 完全二叉树
- 除了叶节点，该树是完全充满的，而且是从左向右填充
- 最大堆，堆顶为其**最大值**
- 最小堆，堆顶为其**最小值**
- 包含$n$个元素的堆的高度为$\lg n$
- 堆结构上的基本操作时间复杂度为$O(\lg n)$

## `Java`实现

!!! tips "`Java`中实现的方式为**优先队列**，通过对元素排序方式的重写，可以分别实现最大堆和最小堆，默认为最小堆。"

    ```Java
    /* 初始化堆 */
    // 初始化小顶堆
    Queue<Integer> minHeap = new PriorityQueue<>();
        // 初始化大顶堆（使用 lambda 表达式修改 Comparator 即可）
    Queue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);

    /* 元素入堆 */
    maxHeap.offer(1);
    maxHeap.offer(3);
    maxHeap.offer(2);
    maxHeap.offer(5);
    maxHeap.offer(4);

    /* 获取堆顶元素 */
    int peek = maxHeap.peek(); // 5

    /* 堆顶元素出堆 */
    // 出堆元素会形成一个从大到小的序列
    peek = heap.poll();  // 5
    peek = heap.poll();  // 4
    peek = heap.poll();  // 3
    peek = heap.poll();  // 2
    peek = heap.poll();  // 1

    /* 获取堆大小 */
    int size = maxHeap.size();

    /* 判断堆是否为空 */
    boolean isEmpty = maxHeap.isEmpty();

    /* 输入列表并建堆 */
    minHeap = new PriorityQueue<>(Arrays.asList(1, 3, 2, 5, 4));

    ```
## 堆的实现

堆是一个**完全二叉树**，所以适合用**数组**来表示

- 给定索引：$i$
- **左**子结点：$2i+1$
- **右**子结点：$2i+2$
- **父**结点：$(i-1)/2$

![](https://p.ipic.vip/otvi0o.png)

## 常用操作

### 1. 元素入堆

!!! quote "元素入堆"

    给定元素 val ，我们先将其添加到堆底。添加后，由于 val 可能大于堆中其它元素，此时堆的成立条件可能已经被破坏，因此需要修复从插入结点到根结点这条路径上的各个结点，该操作被称为「堆化 Heapify」。

    考虑从入堆结点开始，从底至顶执行堆化。具体地，比较插入结点与其父结点的值，若插入结点更大则将它们交换；并循环以上操作，从底至顶地修复堆中的各个结点；直至越过根结点时结束，或当遇到无需交换的结点时提前结束。


    === "1.1"
        ![](https://p.ipic.vip/47awb0.png)

    === "1.2"
        ![](https://p.ipic.vip/4aajzf.png)

    === "1.3"
        ![](https://p.ipic.vip/1e2uyp.png)

    === "1.4"
        ![](https://p.ipic.vip/b1kwe5.png)

    ```Java
    /* 元素入堆 */
    void push(int val) {
        // 添加结点
        maxHeap.add(val);
        // 从底至顶堆化
        siftUp(size() - 1);
    }

    /* 从结点 i 开始，从底至顶堆化 */
    void siftUp(int i) {
        while (true) {
            // 获取结点 i 的父结点
            int p = parent(i);
            // 当“越过根结点”或“结点无需修复”时，结束堆化
            if (p < 0 || maxHeap.get(i) <= maxHeap.get(p))
            break;
            // 交换两结点
            swap(i, p);
            // 循环向上堆化
            i = p;
        }
    }

    ```

### 2. 元素出堆

!!! quote "元素出堆"

    堆顶元素是二叉树根结点，即列表首元素，如果我们直接将首元素从列表中删除，则二叉树中所有结点都会随之发生移位（索引发生变化），这样后续使用堆化修复就很麻烦了。为了尽量减少元素索引变动，采取以下操作步骤：

    - 交换堆顶元素与堆底元素（即交换根结点与最右叶结点）；
    - 交换完成后，将堆底从列表中删除（注意，因为已经交换，实际上删除的是原来的堆顶元素）；
    - 从根结点开始，从顶至底执行堆化；

    顾名思义，从顶至底堆化的操作方向与从底至顶堆化相反，我们比较根结点的值与其两个子结点的值，将最大的子结点与根结点执行交换，并循环以上操作，直到越过叶结点时结束，或当遇到无需交换的结点时提前结束。

    === "2.1"
        ![](https://p.ipic.vip/93aynr.png)

    === "2.2"
        ![](https://p.ipic.vip/9btiz1.png)

    === "2.3"
        ![](https://p.ipic.vip/45jds3.png)

    === "2.4"
        ![](https://p.ipic.vip/m7atgd.png)

    === "2.5"
        ![](https://p.ipic.vip/8v1xuw.png)

    === "2.6"
        ![](https://p.ipic.vip/c59mg3.png)

    === "2.7"
        ![](https://p.ipic.vip/q70oke.png)

    === "2.8"
        ![](https://p.ipic.vip/7a39sh.png)

    === "2.9"
        ![](https://p.ipic.vip/eptnlm.png)

    === "2.10"
        ![](https://p.ipic.vip/irbyno.png)

    ```java
    /* 元素出堆 */
    int pop() {
        // 判空处理
        if (isEmpty())
        throw new EmptyStackException();
        // 交换根结点与最右叶结点（即交换首元素与尾元素）
        swap(0, size() - 1);
        // 删除结点
        int val = maxHeap.remove(size() - 1);
        // 从顶至底堆化
        siftDown(0);
        // 返回堆顶元素
        return val;
    }

    /* 从结点 i 开始，从顶至底堆化 */
    void siftDown(int i) {
        while (true) {
            // 判断结点 i, l, r 中值最大的结点，记为 ma
            int l = left(i), r = right(i), ma = i;
            if (l < size() && maxHeap.get(l) > maxHeap.get(ma))
            ma = l;
            if (r < size() && maxHeap.get(r) > maxHeap.get(ma))
            ma = r;
            // 若结点 i 最大或索引 l, r 越界，则无需继续堆化，跳出
            if (ma == i) break;
            // 交换两结点
            swap(i, ma);
            // 循环向下堆化
            i = ma;
        }
    }

    ```

## 常见应用

!!! note "优先队列"

    堆常作为实现优先队列的首选数据结构，入队和出队操作时间复杂度为$O(\lg n)$，建队操作为$O(n)$，皆非常高效。

!!! note "堆排序"

    给定一组数据，我们使用其建堆，并依次全部弹出，则可以得到有序的序列。当然，堆排序一般无需弹出元素，仅需每轮将堆顶元素交换至数组尾部并减小堆的长度即可。

!!! note "获取最大的$k$个元素"

    这既是一道经典算法题目，也是一种常见应用，例如选取热度前 10 的新闻作为微博热搜，选取前 10 销量的商品等。

## 建堆

!!! tips "借助入堆方法实现"

    最直接地，考虑借助「元素入堆」方法，先建立一个空堆，再将列表元素依次入堆即可。

    设元素数量为$n$  ，则最后一个元素入堆的时间复杂度为 $O(\log n)$ ，在依次入堆时，堆的平均长度为$n/2$，因此该方法的总体时间复杂度为$O(n \log n)$。

!!! tips "基于堆化操作实现"

    先将列表所有元素原封不动添加进堆，然后迭代地对各个结点执行「从顶至底堆化」。当然，无需对叶结点执行堆化，因为其没有子结点。时间复杂度可以达到$O(n)$。


## `leetcode`

### 剑指 Offer 40. 最小的k个数
!!! example "剑指 Offer 40. 最小的k个数"

    输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

    ```java
    // 保持堆的大小为K，然后遍历数组中的数字，遍历的时候做如下判断：
    // 1. 若目前堆的大小小于K，将当前数字放入堆中。
    // 2. 否则判断当前数字与大根堆堆顶元素的大小关系，如果当前数字比大根堆堆顶还大，这个数就直接跳过；
    //    反之如果当前数字比大根堆堆顶小，先poll掉堆顶，再将该数字放入堆中。
    class Solution {
        public int[] getLeastNumbers(int[] arr, int k) {
            if (k == 0 || arr.length == 0) {
                return new int[0];
            }
            // 默认是小根堆，实现大根堆需要重写一下比较器。
            Queue<Integer> pq = new PriorityQueue<>((v1, v2) -> v2 - v1);
            for (int num: arr) {
                if (pq.size() < k) {
                    pq.offer(num);
                } else if (num < pq.peek()) {
                    pq.poll();
                    pq.offer(num);
                }
            }

            // 返回堆中的元素
            int[] res = new int[pq.size()];
            int idx = 0;
            for(int num: pq) {
                res[idx++] = num;
            }
            return res;
        }
    }

    ```

