# 动态规划测试

# 简单难度

## 70. 爬楼梯

得分5

```java
class Solution {
    public int climbStairs(int n) {
        int a = 1,b = 2;
        for(int i=1;i<n;i++){
            int sum = a + b;
            a = b;
            b = sum;
        }
        return a;
    }
}
```

## 118. 杨辉三角

得分4

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> root = new ArrayList<>();
        root.add(1);
        res.add(root);
        for(int i=1;i<numRows;i++){
            List<Integer> temp = new ArrayList<>();
            for(int k=0;k<=i;k++){

                if(k==0||k==i){
                    temp.add(1);
                    continue;
                }else{
                    List<Integer> last = res.get(i-1);
                        temp.add(last.get(k-1)+last.get(k));
                }
            }
            res.add(temp);

        }
        return res;
    }
}
```

## 119. 杨辉三角 II

得分5

```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> res = new ArrayList<>();
        res.add(1);
        for(int i=1;i<=rowIndex;i++){
            List<Integer> temp = new ArrayList<>();
            for(int k=0;k<=i;k++){
                if(k==0||k==i)temp.add(1);
                else temp.add(res.get(k-1)+res.get(k));
            }
            res = temp;
        }
        return res;
    }
}
```

## 121. 买卖股票的最佳时机

得分5

```java
class Solution {
    public int maxProfit(int[] prices) {
        int max = 0, min=Integer.MAX_VALUE;
        for(int price: prices){
            max = Math.max(max,price-min);
            min = Math.min(min,price);

        }
        return max;
    }
}
```

## 392. 判断子序列

得分3

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int m = s.length(), n = t.length();
        int[][] dp = new int[m+1][n+1];
        dp[0][0] = 0;
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(s.charAt(i-1)==t.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1]+1;
                }else {
                    dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[m][n] == m;
    }
}
```

## 509. 斐波那契数

得分5

```java
class Solution {
    public int fib(int n) {
        if(n<2)return n;
        int a=0,b=1,res=0;
        for(int i=2;i<=n;i++){
            res = a + b;
            a = b;
            b = res;
        }
        return res;
    }
}
```

### 746. 使用最小花费爬楼梯

得分0

!!! tip "$为什么dp[0]=0,dp[1]=0$"

    因为从到达$0$和$1$不需要支付之前的费用，它俩是起步阶梯

!!! tip "转移方程怎么判断的"

    1. 规则说了，每次能爬1或者2层，就有$dp(2)$是由$dp(0)$和$dp(1)$选择最优解得到的
    2. 怎么判断的最优解，最少金额支付，那么得到$min(cost(0),cost(1))$
    3. **2**的判断是基于，$0$和$1$是起步阶梯，没有上层了，那么如果是$dp(3)=min(cost(2),cost(1))$，相当于是从$1$或$2$起步了，$1$是起步阶梯没问题，$2$不是，那么$2$之前的子问题怎么获取呢，用$dp(2)$来指代
    4. $dp(3) = min(dp(2)+cost(2),cost(1))$
    5. 由于$dp(0)$和$dp(1)$是起步阶梯，都$=0$，把上面公式改写一下为$dp(3) = min(dp(2)+cost(2),dp(1)+cost(1))$

    ${规律=
        \begin{cases}
        dp(0) = 0&i=0\\
        dp(1) = 0&i=1\\
        dp(2) = min(dp(1)+cost(1),dp(0)+cost(0))&i=2\\
        dp(3) = min(dp(2)+cost(2),dp(1)+cost(1))&i=3\\
        dp(4) = min(dp(3)+cost(3),dp(2)+cost(2))&i=4\\
        ...\\
        dp(i) = min(dp(i-1)+cost(i-1),dp(i-2)+cost(i-2))&i
        \end{cases}
    }$


```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int min = 0;
        int[] dp = new int[cost.length+1];
        dp[0]=0;dp[1]=0;
        for(int i=2;i<=cost.length;i++){
            dp[i] = Math.min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]);
        }
        return dp[cost.length];
    }
}
```


### 1025. 除数博弈

```java
class Solution {
    public boolean divisorGame(int n) {
        boolean[] dp = new boolean[n+1];
        dp[1] = false;
        for(int i=2;i<=n;i++){
            dp[i] = !dp[i-1];
        }
        return dp[n];
    }
}
```

### 1137. 第 N 个泰波那契数

```java
class Solution {
    public int tribonacci(int n) {
        if(n==0)return 0;
        if(n==1||n==2)return 1;
        int[] dp = new int[n+1];
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 1;
        for(int i=3;i<=n;i++){
            dp[i] = dp[i-3]+dp[i-2]+dp[i-1];
        }
        return dp[n];
    }
}
```

# 中等难度

### 5. 最长回文子串

${dp(i,j)=
    \begin{cases}
    true&i==j\\
    true+dp(i+1,j-1)&s[i] == s[j]\\
    false&s[i] \neq s[j]\\
    \end{cases}
}$

```java
class Solution {
    public String longestPalindrome(String s) {
        int length = s.length();
        if(length<2)return s;
        boolean[][] dp = new boolean[length][length];
        for(int i=0;i<length;i++){
            dp[i][i] = true;
        }

        int start = 0;
        int maxLength = 1;
        for(int j=1;j<length;j++){
            for(int i=0;i<j;i++){
                if(s.charAt(i)!=s.charAt(j)){
                    dp[i][j] = false;
                } else {
                    if(j-i<3){
                        dp[i][j] = true;
                    } else {
                        dp[i][j] = dp[i+1][j-1];
                    }
                }
                if(dp[i][j]&&j-i+1 > maxLength){
                    start = i;
                    maxLength = j-i+1;
                }
            }
        }
        return s.substring(start, start+maxLength);
    }
}
```

## 22. 括号生成

```
i = 0结果是空；

i = 1结果有一种：()

i = 2结果有两种：()(), (())

∴i = 3的结果，使用公式：[ + p + ] + q有如下三种情况共5种结果（我以花括号来表示新添加的括号）：

p = 2, q = 0:

{()()}

{(())}

p = 1, q = 1:

{()}()

p = 0, q = 2:

{}()()

{}(())

所以i=3时共5种结果
```

$dp(n,i) = \displaystyle\sum_{i=0,j=0,j<i}^{n} \langle dp(i) \rangle + dp(i-1-j)$


```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<List<String>> res = new ArrayList<>();
        if(n==0)return res.get(0);
        List<String> l0 = new ArrayList<>();
        l0.add("");
        List<String> l1 = new ArrayList<>();
        l1.add("()");
        res.add(l0);
        res.add(l1);
        for(int i=2;i<=n;i++){
            List<String> l = new ArrayList<>();
            for(int j=0;j<i;j++){
                List<String> p = res.get(j);
                List<String> q = res.get(i-1-j);
                for(String p1: p){
                    for(String q1: q){
                        l.add("("+p1+")"+q1);
                    }
                }
            }
            res.add(l);
        }
        return res.get(n);
    }
}
```