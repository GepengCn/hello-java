# 分治算法

## 步骤

1. **分解**：将问题划分为一些子问题，子问题的形式与原问题一样，只是规模更小。
2. **解决**：递归地求解出子问题。如果子问题的规模足够小，则停止递归，直接求解。
3. **合并**：将子问题的解组合成原问题的解。

## 递归式

!!! tip "递归式"

    一个**递归式**就是一个等式或不等式，它通过更小的输入上的函数值来描述一个函数。

三种递归式求解方法：

- **代入法**：猜测一个界，然后用数学归纳法证明这个界是正确的。
- **递归树法**：将递归式转换为一棵树，其结点表示不同层次的递归调用产生的代价。然后采用边界和技术来求解递归式。
- **主方法**：可求解形如下面公式的递归式的界：

    $T(n) = aT(n/b) + f(n)$

    其中$a \geq 1$，$b > 1$，$f(n)$是一个给定的函数。

    它刻画了这样一个分治算法：生成$a$个子问题，每个子问题的规模是原问题规模的$1/b$，分解和合并步骤总共花费时间为$f(n)$。

## leetcode


### 53. 最大子数组和

!!! example "53. 最大子数组和"

    给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

    - [x] 题解

    1. 选取一个中间节点mid
    2. 将问题拆分成3部分，左半部分，右半部分和横跨左右
    3. 然后不断递归向下求解，直到子问题化为最小可以直接求解，然后逐步回溯

    分治将该问题，拆分成三个不同的子问题，子问题递归继续向下拆解，直到可以直接求解

    ```java
    class Solution {
        public int maxSubArray(int[] nums) {
            return dfs(0,nums.length-1,nums);
        }

        public int dfs(int left, int right, int[] nums){
            if(left == right){
                return nums[left];
            }
            int mid = (left + right) >> 1;
            int leftMax = dfs(left, mid, nums);
            int rightMax = dfs(mid+1, right, nums);
            int crossMax = cross(left ,right, nums);

            return Math.max(leftMax, Math.max(rightMax, crossMax));
        }

        public int cross(int left, int right, int[] nums){
            int leftMax = Integer.MIN_VALUE;
            int rightMax = Integer.MIN_VALUE;
            int mid = (left + right) >> 1;
            int sum = 0;
            for(int i=mid;i>=left;i--){
                sum+=nums[i];
                leftMax = Math.max(leftMax, sum);
            }
            sum = 0;
            for(int i=mid+1;i<=right;i++){
                sum+=nums[i];
                rightMax = Math.max(rightMax, sum);
            }
            return leftMax + rightMax;
        }
    }
    ```

### 169. 多数元素

!!! example "169. 多数元素"

    给定一个大小为 n 的数组 nums ，返回其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。

    ```java
    class Solution {
        public int majorityElement(int[] nums) {
            return dfs(0,nums.length-1,nums);
        }
        public int dfs(int lo, int hi, int[] nums){
            if (lo == hi) {
                return nums[lo];
            }
            int mid = (hi - lo) /2 + lo;
            int left = dfs(lo, mid, nums);
            int right = dfs(mid+1,hi, nums);
            if(left==right)return left;
            int lc = count(lo, hi, nums, left);
            int rc = count(lo, hi, nums, right);

            return lc > rc ? left : right;
        }
        public int count(int lo, int hi, int[] nums, int num){
            int total = 0;
            for(int i=lo;i<=hi;i++){
                if(nums[i]==num)total++;
            }
            return total;
        }
    }

    ```