# 数组`Array`

## 思维导图

![](https://p.ipic.vip/v3xymp.png)

## 特性
- `顺序`存储多个`相同类型`的数据, 内存中相连
- 元素在数组中的位置为`索引`
- 有索引，所以访问非常高效
- `固定容量`，一经创建，长度不可变，扩容需要`创建新数组`，将数据`拷贝`过去
- 插入或删除元素效率低，为了使数组的元素相连，可能需要移动其它数组元素，时间复杂度 $O(N)$
- `内存浪费`，初始化一个比较长的数组，只用前面部分

## 初始化

!!! Info "无初始值，但需指定长度"

    ```java
    int[] arr = new int[5]; // { 0, 0, 0, 0, 0 }
    ```
!!! Info "有初始值"

    ```java
    int[] nums = { 1, 3, 2, 5, 4 };
    ```

## 常用操作

!!! Quote "遍历"

    ```java
    int[] nums = { 1, 3, 2, 5, 4 };

    for(int i=0;i<nums.length;i++){
        System.out.println(nums[i]);
    }

    for(int num:nums){
        System.out.println(num);
    }
    ```

!!! Quote "查找"

    ```java
    int a = nums[0];
    ```

!!! Quote "插入"

    ```java
    /* 在数组的索引 index 处插入元素 num */
    void insert(int[] nums, int num, int index) {
        // 把索引 index 以及之后的所有元素向后移动一位
        for (int i = nums.length - 1; i > index; i--) {
            nums[i] = nums[i - 1];
        }
        // 将 num 赋给 index 处元素
        nums[index] = num;
    }
    ```
!!! Quote "删除"

    ```java
    /* 删除索引 index 处元素 */
    void remove(int[] nums, int index) {
        // 把索引 index 之后的所有元素向前移动一位
        for (int i = index; i < nums.length - 1; i++) {
            nums[i] = nums[i + 1];
        }
    }

    ```

## 实际应用

!!! Note "随机访问"

    如果我们想要随机抽取一些样本，那么可以用数组存储，并生成一个随机序列，根据索引实现样本的随机抽取。

!!! Note "二分查找"

    例如查字典的例子，我们可以将字典中的所有字按照拼音顺序存储在数组中，然后使用与日常查纸质字典相同的“翻开中间，排除一半”的方式，来实现一个查电子字典的算法。

!!! Note "深度学习"

    神经网络中大量使用了向量、矩阵、张量之间的线性代数运算，这些数据都是以数组的形式构建的。数组是神经网络编程中最常使用的数据结构。

## `leetcode`

!!! example "1. 两数之和"

    给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

    你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

    你可以按任意顺序返回答案。

    示例 1：

    ```txt
    输入：nums = [2,7,11,15], target = 9
    输出：[0,1]
    解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

    ```

    ```java
    class Solution {
        public int[] twoSum(int[] nums, int target) {
            for(int i=0;i<nums.length;i++){
                for(int j=i+1;j<nums.length;j++){
                    if(nums[i]+nums[j]==target){
                        int[] res = {i,j};
                        return res;
                    }
                }
            }
            return new int[0];
        }
    }
    ```