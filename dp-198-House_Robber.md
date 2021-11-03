### 思路

- 拿到题的直觉解法
    - dp没练过，每次都要好好练一下思路；
    - 关键是找到dp函数，一种递推关系；
    - 那如果从左往右想，将函数F(i)设定为，当前位置及以后的最优决策，那么问题就转化为了一个求max；
    ```
       最优解 = max(choice_0, choice_1...choice_n-1) 
       choice_0 = F(0) = nums[0] + F(2), 如果抢第0家
       choice_1 = F(1) = nums[1] + F(3), 如果抢第1家
       choice_2 = F(2) = nums[2] + F(4), 如果抢第2家
       ...
       choice_n-3 = F(n-3) = nums[n-3] + F(n-1), 如果抢第n-3家
       choice_n-2 = F(n-2) = nums[n-2] + F(n), 如果抢第n-2家
       choice_n-1 = F(n-1) = nums[n-1] + F(n+1), 如果抢第n-1家
    ```
    - 那么只需要从最后往前递推就可以，这种算是top-down的解法吧

`AC`


### 代码
```java
//从左往右想，从右往左推，top-down，递归
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n+2];
        int res = 0;
        for(int i = n + 1;i >= 2;i--){
            dp[i - 2] = Math.max(nums[i - 2] + dp[i], res);
            res = dp[i - 2];
        }
        
        return res;
    }
}
```


### 复杂度

time: O(N)，遍历每个step
space: O(N)，都需要，存储dp值，其实可以优化到O(1)，因为每一次都只是想知道next.next的值，而不是全部。
