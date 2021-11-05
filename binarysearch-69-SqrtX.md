### 思路

- 拿到题的直觉解法
    - 二分的题，目标是将题目匹配到模板；
    - 没匹配，就没想清楚，做出来也没意义；下次还是错；
    - Sqrt不一定能找到刚好等于的数，对于n=4来说，是个精确匹配，但对于n=8来说，问题的性质都变了；
    - 根据题目条件，省略小数，综合考虑，匹配模板：
    `寻找最右插入位置；也就是最接近，但不超过sqrt(n)的值`

`AC`


### 代码
```java
class Solution {
    public int mySqrt(int x) {
        int left = 0, right = x;
        
        while(left <= right){
            int mid = left + (right - left) / 2;
            if((long)mid * mid <= x){
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        if(right < 0 || (long)right * right > x){
            return -1;
        }
        
        return right;
    }
}
```


### 复杂度

time: O(logN)
space: O(1)