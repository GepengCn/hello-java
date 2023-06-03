# Leetcode尊享面试100题

## 624. 数组列表中的最大距离

动态规划

dp(i) = Math.max(max,Math.max(Math.abs(a[i][n]-min),Math.abs(max-a[i][0])))