### 思路

- 拿到题的直觉解法
    - 题目不难，关键是匹配模版；
    - 找最右插入位置或最左符合条件的值-1

`AC`


### 代码
```java
class Solution {
    public int arrangeCoins(int n) {
        long left = 0, right = 80000;
        
        while(left <= right){
            long mid = left + (right - left) / 2;
            
            if(sum(mid) > n){
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return (int)left-1;
    }
    
    public long sum(long n){
        return (1 + n) * n / 2;
    }
}
```


### 复杂度

time: 
- 排序：O(logN)
- 二分：O(1)