# 贪心算法

## 特性

- 解决最优化问题
- 对于许多问题，相比于动态规划，更简单、更高效
- 总是做出局部最优解，寄希望这样的选择导致全局最优解
- 并不保证最优解
- 动态规划需要求出所有子问题的最优解，贪心算法只需要一个选择（贪心的选择）

## leetcode


### 455. 分发饼干

!!! tip "455. 分发饼干"

    将胃口与饼干尺寸排序，使得最小胃口的小孩用最小尺寸的饼干去满足，最后得到最大的收益

    ```java
    class Solution {
        public int findContentChildren(int[] g, int[] s) {
            Arrays.sort(g);
            Arrays.sort(s);
            int i=0,j=0;
            while(i<g.length&&j<s.length){
                if(g[i]<=s[j]){
                    i++;
                }
                j++;
            }

            return i;
        }
    }
    ```

### 605. 种花问题

!!! tip "605. 种花问题"

    用贪心思想，不打破种植规律的情况下，种植尽可能多的花，最后判断种入的花的最多数量是否大于n

    ```java

    class Solution {
        public boolean canPlaceFlowers(int[] flowerbed, int n) {
            for(int i=0;i<flowerbed.length;i++){
                if(flowerbed[i]==0&&(i+1==flowerbed.length||flowerbed[i+1]==0)){
                    n--;
                    i++;
                } else if(flowerbed[i]==1){
                    i++;
                }
            }
            return n<=0;
        }
    }
    ```