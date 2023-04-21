# 动态规划测试

## 70. 爬楼梯

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