# 动态规划

![](https://p.ipic.vip/utwjtb.jpg)

## 原理

- 将递归算法重写成非递归算法，让后者把那些子问题的答案系统地记录起来
- 递归会造成冗余计算，已解过的子问题无法再利用


## 与分治算法比较

!!! tip "与分治算法比较"

    - [x] 分治算法

    将问题划分为互不相交的子问题，递归地求解子问题，再将它们的解组合起来，求出原问题的解。

    - [x] 动态规划

    应用于子问题重叠的情况，即不同的子问题具有公共的子子问题。

    - [x] 子问题重叠
        - 在这种情况下，分治算法会做许多不必要的工作，它会反复地求解那些公共子子问题。
        - 动态规划对每个子子问题只求解一次，将其解保存在一个表格中，从而无需每次求解一个子子问题时都重新计算。

## 设计动态规划算法步骤

1. 刻画一个最优解的结构特征
2. 递归地定义最优解的值。
3. 计算最优解的值，通常采用自底而上的方法。
4. 利用计算出的信息构造一个最优解。

## 最优子结构

问题的最优解由相关子结构问题的最优解组合而成，而这些子问题可以独立求解

## 两种等价方法

1. 带备忘的自顶向下法（递归）

2. 自底向上法（迭代）

## 什么时候使用

### 1. 最优子结构

一个问题的最优解包含其**子问题**的最优解

!!! info "怎么发掘最优子结构？"

    1. 做出一个选择
    2. 已知最简子问题的最优选择
    3. 逐步递进选择后，刻画子问题空间（找出规律,尽可能简单，只在必要时扩展）
    4. 积累最优子问题直到目标问题完全由最优子问题构成，此时，目标问题已是最优选择

!!! info "怎么区分不同问题的最优子结构"

    1. 多少个子问题
    2. 每个子问题需要考察多少种选择

    所以时间复杂度即为 **子问题数量** $\times$ **考察多少选择**

    如果$n$个子问题，每个子问题需要做出$n$个选择，时间复杂度为$O(n^2)$

    也可用**图**来做同样的分析。

    1. 图中每个**顶点**对应一个**子问题**
    2. 需要考察的**选择**对应关联至子问题顶点的**边**

!!! info "与贪心算法相比"

    - 相似之处：在于也必须具有**最优子结构**性质
    - 不同之处：贪心算法并不是首先寻找子问题的最优解，然后再其中进行选择，而是首先做出一次**贪心**选择 $\implies$ 在当时看来最优的选择 $\implies$ 然后求解选出的子问题，从而不必费心求解所有可能相关的子问题

!!! info "怎么看问题是否具有最优子结构性质"

    子问题之间无关，同一个原问题的一个子问题的解不影响另一个子问题的解，类似于图的顶点和边被某个子问题占用就不能在被其它子问题占用。

    换个角度来看，求解某个子问题的用到了某些资源（比如顶点），再解其它子问题时不可用，或者说资源不共享，是独占的。


### 2. 子问题重叠

子问题空间必须足够**小**，即问题的递归算法会反复地求解相同的子问题，而不是一直生成新的子问题。

与之相对的，适合用分治方法求解的问题通常在递归的每一步都生成全新得子问题。

!!! info "子问题重叠在动态规划中的使用"

    对每个子问题求解一次，将解存入一个表中，当再次需要这个子问题时直接查表，每次查表的代价为常量时间。


!!! info "自底向上动态规划与自顶向下备忘算法（递归）"

    - 如果每个问题都必须至少求解一次，动态规划要更快，虽然都是$O(n^3)$（但是相差一个常量系数），但是递归调用有开销，表的维护开销也更小

    - 如果不是每个问题都需要求解，只求解必要的子问题，那备忘方法更有优势

## 最长公共子序列

### 刻画最长公共子序列的特征


${c[i,j]=
\begin{cases}
0 & \quad \text{ 若 } i = 0 \text{ 或 } j = 0\\
c[i-1,j-1] + 1 & \quad \text{ 若 } i,j > 0 \text{ 且 } x_i = y_i\\
max(c[i,j-1],c[i-1,j]) & \quad \text{ 若 } i,j > 0 \text{ 且 } x_i \neq y_i
\end{cases}
}$

<div class="center-table" markdown>
![](https://p.ipic.vip/3vc3ff.jpg){width=300}
</div>


## leetcode

### 剑指 Offer 10- I. 斐波那契数列

=== "1.1刻画一个最优解的结构特征"

    ${
        规律=
        \begin{cases}
        f(0)=0\\
        f(1) = 1\\
        f(2) = f(0) + f(1) = 1\\
        f(3) = f(1) + f(2) = 2\\
        f(4) = f(2) + f(3) = 3\\
        f(5) = f(3) + f(4) = 5\\
        ...\\
        f(n) = f(n-1) + f(n-2)
        \end{cases}
    }$




=== "1.2递归地定义最优解的值"

    $dp(i) = dp(i-2) + dp(i-1)$

=== "1.3计算最优解的值，通常采用自底而上的方法"

    ```java
    class Solution {
        public int fib(int n) {
            if(n <= 1) return n;

            int a = 0,b = 1, sum = 0;

            for(int i = 2;i <= n;i++){
                sum = (a + b)%1000000007;
                a = b;
                b = sum;
            }

            return sum;
        }
    }

    ```

### 剑指 Offer 10- II. 青蛙跳台阶问题

=== "2.1刻画一个最优解的结构特征"

    ${规律=
    \begin{cases}
    f(2) = 11 2 = 2\\
    f(3) = 111 12 21 = 3\\
    f(4) = 1111 211 121 112 22 = 5\\
    f(5) = 11111 2111 1211 1121 1112 122 212 221= 8\\
    f(6) = 13\\
    f(7) = 21\\
    f(n) = f(n-1) + f(n-2)
    \end{cases}
    }$

=== "2.2递归地定义最优解的值"

    $dp(n) = dp(n-1) + dp(n-2)$


=== "2.3计算最优解的值，通常采用自底而上的方法"

    ```java
    class Solution {
        public int numWays(int n) {
            if(n<=2)return n;
            int a = 1, b = 2, sum = 0;

            for(int i=3;i<=n;i++){
                sum = a + b;
                a = b;
                b = sum;
            }
            return sum;
        }
    }

    ```

### 剑指 Offer 63. 股票的最大利润

=== "3.1刻画一个最优解的结构特征"

    `输入[7,1,5,3,6,4]`

    ${规律=
        \begin{cases}
        第一天买，第二天卖，-6\\
        第一天买，第三天卖，-2\\
        第一天买，第四天卖，-4\\
        第一天买，第五天卖，-1\\
        第一天买，第六天卖，-3\\
        i 是买入 j 是卖出，上面的第一天买，第二天卖就是i=1,j=2\\
        i=2,j=3,4\\
        i=2,j=4,2\\
        i=2,j=5,5\\
        i=2,j=6,3\\
        ...\\
        i=5,j=6,-2\\
        i=6,跳出循环\\
        第一天买,最大利润,是不买不卖=0\\
        第二天买,最大利润,是第五天卖=5\\
        第三天买,最大利润,是第六天卖=1\\
        第四天买,最大利润,是第六天卖=3\\
        第五天买,最大利润,是不买不卖=0\\
        最大利润是，前i-1天的最大利润与第i天的价格减去前i-1天的最小价格作比较，取较大值
        \end{cases}
    }$

=== "3.2递归地定义最优解的值"

    $dp(i) = max(dp(i-1), prices(i)-min(prices(i-1)))$

=== "3.3计算最优解的值，通常采用自底而上的方法"

    循环一次，每天都更新最小价格和最大利润

    ```java
    class Solution {
        public int maxProfit(int[] prices) {
            if(prices==null||prices.length==0)return 0;
            int max = 0, min = Integer.MAX_VALUE;
            for(int i=0;i<prices.length;i++){
                min = Math.min(min, prices[i]);
                max = Math.max(max, prices[i]-min);
            }
            return max;
        }
    }
    ```

    ### 剑指 Offer 42. 连续子数组的最大和

    === "4.1刻画一个最优解的结构特征"

    输入: `nums = [-2,1,-3,4,-1,2,1,-5,4]`
    ${规律=
        \begin{cases}
        [-2]&最大和为{-2}&子数组=[-2]\\
        [-2,1]&最大和为1&子数组=[1]\\
        [-2,1,-3]&最大和为1&子数组=[1]\\
        [-2,1,-3,4]&最大和为4&子数组=[4]\\
        [-2,1,-3,4,-1]&最大和为4&子数组=[4]\\
        [-2,1,-3,4,-1,2]&最大和为5&子数组=[4,-1,2]\\
        [-2,1,-3,4,-1,2,1]&最大和为6&子数组=[4,-1,2,1]\\
        [-2,1,-3,4,-1,2,1,-5]&最大和为6&子数组=[4,-1,2,1]\\
        [-2,1,-3,4,-1,2,1,-5,4]&最大和为6&子数组=[4,-1,2,1]\\
        \end{cases}
    }$

    === "4.2递归地定义最优解的值"

    ${dp(i)=
        \begin{cases}
        nums[i]&dp(i-1)<=0\\
        dp(i-1) + nums[i]&dp(i-1)>0
        \end{cases}
    }$

    === "4.3计算最优解的值，通常采用自底而上的方法"

    ```java
    class Solution {
        public int maxSubArray(int[] nums) {
            int last = Integer.MIN_VALUE;
            int max = last;
            for(int num: nums){
                if(last<=0)last = num;
                else last += num;
                max = Math.max(max, last);
            }
            return max;
        }
    }
    ```

### 剑指 Offer 47. 礼物的最大价值

=== "5.1刻画一个最优解的结构特征"
    输入:
    ```
    [
        [1,3,1],
        [1,5,1],
        [4,2,1]
    ]
    ```
    ${规律=
        \begin{cases}
        [1*1]&最大和为1&路径为1\\
        [1*2]&最大和为4&路径为1\to3\\
        [1*3]&最大和为5&路径为1\to3\to1\\
        [2*1]&最大和为2&路径为1\to1\\
        [2*2]&最大和为9&路径为1\to3\to5\\
        [2*3]&最大和为10&路径为1\to3\to5\to1\\
        [3*1]&最大和为6&路径为1\to1\to4\\
        [3*2]&最大和为11&路径为1\to3\to5\to2\\
        [3*3]&最大和为12&路径为1\to3\to5\to2\to1\\
        \end{cases}
    }$


=== "5.2递归地定义最优解的值"
    ${dp(i,j)=
        \begin{cases}
        grid[i][j]&i = 0,j = 0\\
        grid[i][j] + dp(i-1,j)&i \neq 0,j = 0\\
        grid[i][j] + dp(i,j-1)&i = 0,j \neq 0\\
        grid[i][j] + max[dp(i-1,j),dp(i,j-1)]&i \neq 0,j \neq 0\\
        \end{cases}
    }$

=== "5.3计算最优解的值，通常采用自底而上的方法"


    - `grid`随着m*n次遍历后，每个格子分别存了对应的最大价值
    - 本题将子问题的最优解，保存到格子$grid[i][j]$中,这样随着格子`i++`,`j++`增大，直接复用$grid[i][j]$的最优解，而不必重复计算
        - [x] 就比如格子由`[1 * 2]`扩张到`[1 * 3]`时，${dp(1,3) =
        \begin{cases}
        dp(1,2) + grid[0,2]&第一步\\
        grid[0,1] + grid[0,2]&第二步\\
        4 + 1&第三步,这个4是dp(1,2)的最优解，已缓存在格子里了\\
        5&第四步\\
        \end{cases}
        }$

    ```java
    class Solution {
        public int maxValue(int[][] grid) {
             int m = grid.length;
            int n = grid[0].length;
            for(int i=0;i<m;i++){
                for(int j=0;j<n;j++){
                    if(i==0&&j==0) continue;
                    else if(i==0) grid[i][j] += grid[i][j-1];
                    else if(j==0) grid[i][j] += grid[i-1][j];
                    else grid[i][j] += Math.max(grid[i-1][j],grid[i][j-1]);
                }
            }
            return grid[m-1][n-1];
        }
    }
    ```
### 剑指 Offer 46. 把数字翻译成字符串

=== "6.1刻画一个最优解的结构特征"

    12258
    ${规律=
        \begin{cases}
        1&b&f(1) = 1\\
        12&bc,m&f(2) = 2\\
        122&bcc,bw,mb&f(3) = 3\\
        1225&bccf,mz,bwf,bcz,mcf&f(4) = 5\\
        12258&bccfi,mcfi,bwfi,bczi,mzi&f(4) = 5\\
        \end{cases}
    }$

    - 新增的元素如果不能跟上一个元素组成新元素，可翻译的字符串数量不会增加
    - f(n) = f(n-1) + f(n-2)

=== "6.2递归地定义最优解的值"
    ${dp(i)=
        \begin{cases}
        dp(i-1)&str[i-1,i]匹配\\
        dp(i-1) + dp(i-2)&不匹配\\
        \end{cases}
    }$

=== "6.3计算最优解的值，通常采用自底而上的方法"

    ```java
    class Solution {
        public int translateNum(int num) {
            String s = num + "";
            int sum = 1;
            int last = 1, pre = 1;
            for(int i=2;i<=s.length();i++){
                String str = s.substring(i-2,i);
                if(str.compareTo("10")<0||str.compareTo("25")>0){
                    sum = pre;
                }else {
                    sum = last + pre;
                }
                last = pre;
                pre = sum;
            }
            return sum;
        }
    }
    ```

### 剑指 Offer 48. 最长不含重复字符的子字符串

=== "7.1刻画一个最优解的结构特征"

    输入: "abcabcbb"

    输入: "bbbbb"

    输入: "pwwkew"

    !!! tip "规律分析"

        1. 衡量子字符串长度需要使用双指针$i$和$j$，$i$是起始指针，$j$是终止指针，长度是$j-i$也即$dp(j)$
        2. 遍历顺序是从左即右的，所以要逐步增大$j$指针，这时候就需要根据新$j$的值，判断i的落点，因为有可能$j$的值，与字符串内部的值重合，这时候，$i$指针也要往前递进
        3. 随着$i,j$指针交替向前，最大长度也随之改变，直到遍历完

        - [x] **怎么确认最优解的**

            最大的$j-i$即是最优解，可能有多个，所以有$dp(j) = j - i$

        - [x] **递归的规律是怎么变化的**

            1. `j++`，这里可以使用for循环
            2. j增加后，新值str[j]是否存在于字符串？
                - 存在，$i$移动到已存在的字符位置，即i=str.getIndex(str[j])，此时$j-i$也发生变化（会减小）
                - 不存在，说明新增的字符依然是不重复字符，可以继续递增，即$dp(j) = dp(j-1) + 1$，此时$j-i$也发生变化（会增大）
            3. 创建一个`max`变量，缓存递归过程中的最大$j-i$值，直到递归完成，返回`max`即为结果


    ${规律=
        \begin{cases}
        3&abc\\
        1&b\\
        3&wke\\
        \end{cases}
    }$

=== "7.2递归地定义最优解的值"
    ${dp(j)=
        \begin{cases}
        dp(j-1) + 1& dp(j-1) < j - i\\
        j - i&dp(j-1) \geq j - i\\
        \end{cases}
    }$

=== "7.3计算最优解的值，通常采用自底而上的方法"

    ```java
    class Solution {
        public int lengthOfLongestSubstring(String s) {
            int tmp = 0, res = 0;
            Map<Character, Integer> dict = new HashMap<>();
            for(int j=0;j<s.length();i++){
                int i = dict.getOrDefault(s.charAt(j),-1);
                dict.put(s.charAt(j),j);
                tmp = tmp < j-i ? tmp + 1 : j - i;
                res = Math.max(res,tmp);
            }

            return res;
        }
    }

    ```

### 剑指 Offer 14- I. 剪绳子

=== "8.1刻画一个最优解的结构特征"

    ${规律=
        \begin{cases}
        1&f(1)\\
        1\times1=1&f(2)\\
        1\times2=2&f(3)\\
        2\times2=4&f(4)\\
        2\times3=6&f(5)\\
        3\times3=9&f(6)\\
        2\times2\times3=12&f(7)\\
        2\times3\times3=18&f(8)\\
        3\times3\times3=27&f(9)\\
        2\times2\times3\times3=36&f(10)\\
        \end{cases}
    }$
=== "8.2递归地定义最优解的值"
    假设长度为i的绳子，剪的第一刀长度为$k$，那么剩余的长度为$i-k$
    ${dp(i)=
        \begin{cases}
        k \times (i-k)&i-k剩余的长度不能再剪(0或1)\\
        k \times dp(i-k)&剩余的长度可再剪\\
        \end{cases}
    }$

=== "8.3计算最优解的值，通常采用自底而上的方法"

    ```java
    class Solution {
        public int cuttingRope(int n) {
            int[] dp = new int[n+1];
            for(int i=2;i<=n;i++){
                int max = 0;
                for(int k=1;k<i;k++){
                    if(i-k>3){
                        max = k * dp[i-k];
                    } else {
                        max = k * (i - k);
                    }
                    dp[i] = Math.max(dp[i],max);
                }
            }
            return dp[n];
        }
    }
    ```

### 剑指 Offer 62. 圆圈中最后剩下的数字

=== "9.1刻画一个最优解的结构特征"

    ${规律=
        \begin{cases}
        f(1) = 0&0\\
        f(2) = 0,1&1\\
        f(3) = 2,0,1&1\\
        f(4) = 2,1,3,0&0\\
        f(5) = 2,0,4,1,3&3\\
        f(6) = 2,5,3,1,4,0&0\\
        \end{cases}
    }$

=== "9.2递归地定义最优解的值"

    约瑟夫环问题

    $f(n) = (f(n-1) + m) % n$

    推导

    $dp(n) = (dp(n-1) + m) % n$

=== "9.3计算最优解的值，通常采用自底而上的方法"

    ```java
    class Solution {
        public int lastRemaining(int n, int m) {
            int x = 0;
            for(int i = 1;i <= n;i++){
                x = (x + m) % i;
            }
            return x;
        }
    }
    ```

### 剑指 Offer 19. 正则表达式匹配

=== "10.1刻画一个最优解的结构特征"
    https://leetcode.cn/problems/zheng-ze-biao-da-shi-pi-pei-lcof/solution/by-flix-ziew/

=== "10.2递归地定义最优解的值"

    $p[j-1] = '*'$

    ${dp[i][j]=
        \begin{cases}
       dp[i][j-2]& 0匹配\\
       dp[i-1][j]& s[i-1]=p[j-2]，匹配1次\\
       dp[i-1][j]& p[j-2]==.\\
        \end{cases}
    }$

    $p[j-1] \neq '*'$

    ${dp[i][j]=
        \begin{cases}
        false& s[i-1]=p[i-1]，不匹配\\
        dp[i-1][j]& s[i-1]=p[i-1]，匹配\\
        dp[i-1][j]& p[j-1]==.\\
        \end{cases}
    }$




=== "10.3计算最优解的值，通常采用自底而上的方法"

    ```java
    class Solution {
        public boolean isMatch(String s, String p) {
            int m = s.length() + 1;
            int n = p.length() + 1;
            boolean[][] dp = new boolean[m][n];
            dp[0][0] = true;

            for(int j=2;j<n;j+=2){
                dp[0][j] = dp[0][j-2] && p.charAt(j-1) == '*';
            }

            for(int i=1;i<m;i++){
                for(int j=1;j<n;j++){
                    if(p.charAt(j-1)=='*'){
                        if(dp[i][j-2]){
                            dp[i][j] = true;
                        } else if(s.charAt(i-1) == p.charAt(j-2)){
                            dp[i][j] = dp[i-1][j];
                        } else if(p.charAt(j-2) == '.'){
                            dp[i][j] = dp[i-1][j];
                        }
                    } else {
                        if(s.charAt(i-1) == p.charAt(j-1)){
                            dp[i][j] = dp[i-1][j-1];
                        } else if(p.charAt(j-1) == '.'){
                            dp[i][j] = dp[i-1][j-1];
                        }
                    }

                }
            }

            return dp[m-1][n-1];
        }
    }

    ```