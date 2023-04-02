# 冒泡排序`Bubble Sort`

## 思维导图

![](https://p.ipic.vip/5hjjby.png)

## 特性

- 一种基于元素交换实现排序的算法

## 算法流程

!!! tip "算法流程"

    「冒泡操作」则是在模拟上述过程，具体做法为：从数组最左端开始向右遍历，依次对比相邻元素大小，若“左元素 $>$ 右元素”则将它俩交换，最终可将最大元素移动至数组最右端。

    完成一次冒泡操作后，数组最大元素已在正确位置，接下来只需排序剩余 $n-1$ 个元素。
    
    > 一次冒泡
    
    === "1.1"
    
        ![](https://p.ipic.vip/ro5gmz.png){width=300}
        
    === "1.2"
    
        ![](https://p.ipic.vip/eknptk.png){width=300}
        
    === "1.3"
    
        ![](https://p.ipic.vip/fy8g4c.png){width=300}
        
    === "1.4"
    
        ![](https://p.ipic.vip/e5u3ea.png){width=300}
        
    === "1.5"
    
        ![](https://p.ipic.vip/9vqixk.png){width=300}
        
    > 冒泡全流程
        
    ![](https://p.ipic.vip/zh3cgz.png)
    
    > 代码
    
    ```java
    /* 冒泡排序 */
    void bubbleSort(int[] nums) {
        // 外循环：待排序元素数量为 n-1, n-2, ..., 1
        for (int i = nums.length - 1; i > 0; i--) {
            // 内循环：冒泡操作
            for (int j = 0; j < i; j++) {
                if (nums[j] > nums[j + 1]) {
                    // 交换 nums[j] 与 nums[j + 1]
                    int tmp = nums[j];
                    nums[j] = nums[j + 1];
                    nums[j + 1] = tmp;
                }
            }
        }
    }
    ```
    
## 复杂度分析

**时间复杂度 $O(n^2)$** ：各轮冒泡遍历的数组长度为 $n - 1$ , $n - 2$ , $\cdots$ , $2$ , $1$ 次，求和为 $\frac{(n - 1) n}{2}$ ，因此使用 $O(n^2)$ 时间。引入下文的 `flag` 优化后，最佳时间复杂度可以达到 $O(N)$ 。

**空间复杂度 $O(1)$** ：指针 $i$ , $j$ 使用常数大小的额外空间。

## 效率优化

若在某轮「冒泡」中未执行任何交换操作，则说明数组已经完成排序，可直接返回结果。考虑可以增加一个标志位 `flag` 来监听该情况，若出现则直接返回。

优化后，冒泡排序的最差和平均时间复杂度仍为 $O(n^2)$ ；而在输入数组完全有序时，达到最佳时间复杂度 $O(n)$ 。

```java
/* 冒泡排序（标志优化）*/
void bubbleSortWithFlag(int[] nums) {
    // 外循环：待排序元素数量为 n-1, n-2, ..., 1
    for (int i = nums.length - 1; i > 0; i--) {
        boolean flag = false; // 初始化标志位
        // 内循环：冒泡操作
        for (int j = 0; j < i; j++) {
            if (nums[j] > nums[j + 1]) {
                // 交换 nums[j] 与 nums[j + 1]
                int tmp = nums[j];
                nums[j] = nums[j + 1];
                nums[j + 1] = tmp;
                flag = true;  // 记录交换元素
            }
        }
        if (!flag) break;     // 此轮冒泡未交换任何元素，直接跳出
    }
}

```