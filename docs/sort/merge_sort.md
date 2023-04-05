# 归并排序 `Merge Sort`

## 思维导图

![](https://p.ipic.vip/bnam03.jpg)

## 特性

- 归并：将两个有序的数组归并成一个更大的数组
- 递归排序算法
- 时间复杂度$O(n \log n)$，空间复杂度$O(n)$

## 原地归并

!!! tip "原地归并"

    先将所有元素复制到`aux[]`中，然后再归并回`a[]`中。
    方法再归并时，进行了4个条件判断：

    1. 左半边用尽（取右半边元素）
    2. 右半边用尽（取左半边元素）
    3. 右半边的当前元素小于左半边的当前元素（取右半边元素）
    4. 左半边的当前元素小于右半边的当前元素（取左半边元素）

    ![](https://p.ipic.vip/5j6yu0.png)

    ```java
    public static void merge(int[] a, int lo, int mid, int hi){
        //将a[lo..mid] 和 a[mid..hi]归并
        int i = lo, j = mid+1;
        //将a[lo..hi]复制到aux[lo..hi]
        for(int k=lo;k<=hi;k++)
            aux[k] = a[k];
        //归并回到a[lo..hi]
        for(int k=lo;k<=hi;k++){
            if(i>mid)                   a[k] = aux[j++];//左半边用尽（取右半边元素）
            else if(j>hi)               a[k] = aux[i++];//右半边用尽（取左半边元素）
            else if(less[aux[j],aux[i]) a[k] = aux[j++];//右半边的当前元素小于左半边的当前元素（取右半边元素）
            else                        a[k] = aux[i++];//左半边的当前元素小于右半边的当前元素（取左半边元素）
        }
    }
    ```

## 自顶向下的归并排序

!!! tip "自顶向下的归并排序"

    - 分治思想
    - 递归
    - 如果它能将两个子数组排序，它就能够通过归并两个子数组来将整个数组排序

    ![](https://p.ipic.vip/l2nys2.jpg)

    <div class="center-table" markdown>
    ![](https://p.ipic.vip/mhvzx8.jpg){width=200}
    </div>

    ```java
    public class Merge{
        private static int[] aux; //归并所需的辅助数组
        public static void sort(int[] a){
            aux = new int[a.length];//一次性分配空间
            sort(a, 0, a.length - 1);
        }
        private static void sort(int[] a, int lo, int hi){
            //将数组a[lo..hi]排序
            if(hi <= lo) return;
            int mid = lo + (hi - lo)/2;
            sort(a, lo, mid);//将左半边排序
            sort(a, mid+1, hi);//将右半边排序
            merge(a, lo, mid, hi);//归并结果
        }
    }

    ```

## 算法优化

### 对小规模子数组使用插入排序
!!! info "对小规模子数组使用插入排序"

    比如长度小于15的子数组，使用插入排序

### 测试数组是否有序

!!! info "测试数组是否有序"

    添加一个判断条件，如果`a[mid]<a[mid+1]`，就可以认定数组是有序的了，不必执行`merge`方法

### 不将元素复制到辅助数组

!!! info "不将元素复制到辅助数组"

    - 将数据从输入数组排序到辅助数组
    - 将数据从辅助数组排序到输入数组


## 自低向上的归并排序


!!! tip "自低向上的归并排序"

    先归并那些微型数组，然后再成对归并得到的子数组，如此这般，直到我们将整个数组归并在一起


    ```java
    public class MergeBU {
        private static int[] aux;//归并所需的辅助数组

        public static void sort(int[] a){
            //进行lgN次两两归并
            int N = a.length;
            aux = new int[N];
            for(int sz = 1;sz < N; sz = sz + sz){//sz子数组大小
                for(int lo = 0; lo < N - sz; lo += sz +sz){//lo: 子数组索引
                    merge(a, lo, lo+sz-1, Math.min(lo+sz+sz-1, N-1));
                }
            }
        }
    }
    ```

    自底向上的归并排序会多次遍历整个数组，根据子数组大小进行两两归并。子数组的大小`sz`的初始值为$1$，每次加倍，最后一个子数组的大小只有在数组大小是`sz`的偶数倍的时候才会等于`sz`(否则它会比`sz`小)。

    <div class="center-table" markdown>
    ![](https://p.ipic.vip/qjwfs1.png){width=350}
    </div>

## 复杂度分析


**时间复杂度 $O(n \log n)$** ：划分形成高度为 $\log n$ 的递归树，每层合并的总操作数量为 $n$ ，总体使用 $O(n \log n)$ 时间。

**空间复杂度 $O(n)$** ：需借助辅助数组实现合并，使用 $O(n)$ 大小的额外空间；递归深度为 $\log n$ ，使用 $O(\log n)$ 大小的栈帧空间，因此是“非原地排序”。

在合并时，不改变相等元素的次序，是“稳定排序”。

## 链表排序 *

归并排序有一个很特别的优势，用于排序链表时有很好的性能表现，**空间复杂度可被优化至 $O(1)$** ，这是因为：

- 由于链表可仅通过改变指针来实现结点增删，因此“将两个短有序链表合并为一个长有序链表”无需使用额外空间，即回溯合并阶段不用像排序数组一样建立辅助数组 `tmp` ；
- 通过使用「迭代」代替「递归划分」，可省去递归使用的栈帧空间；

> 详情参考：[148. 排序链表](https://leetcode-cn.com/problems/sort-list/solution/sort-list-gui-bing-pai-xu-lian-biao-by-jyd/)