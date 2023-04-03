# 选择排序 `Selection Sort`

## 思维导图

![](https://p.ipic.vip/b6flh3.jpg)

## 特性

- 不断选择剩余元素之中的最小者
- 时间复杂度$O(n^2)$，空间复杂度$O(1)$，交换次数为$O(n)$,最好为$0$，最差为$n-1$
- 运行时间和输入无关
- 数据移动是最少的

## 算法流程

!!! tip "算法流程"

    1. 找到数组中最小的元素
    2. 将它和数组的第一个元素交换位置（如果第一个元素就是最小元素那么它就和自己交换）
    3. 在剩下的元素中找到最小的元素，将它和数组的第二个元素交换位置
    4. 如此往复，直到将整个数组排序
    
    ```java
    public class Selection{
        public static void sort(int[] a){
            int N = a.length;//数组长度
            for(int i=0;i<N;i++){
                //将a[i]和a[i+1..N]中的最小的元素交换
                int min = i;
                for(int j=i+1;j<N;j++){
                    if(less(a[j],a[min]))min = j;
                }
                exch(a, i, min);
            }
        }    
    }
    
    ```