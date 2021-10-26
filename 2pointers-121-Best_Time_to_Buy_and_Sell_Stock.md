### 思路

- 拿到题的直觉解法
    - bf, 然而O(n^2)，导致`TLE`
- 看答案最优解
    - 双指针从左往右
    - 左指针指向当前min，有指针往右，随时更新max profit

`AC`


### 代码
```java
//bf
class Solution {
    public int maxProfit(int[] prices) {
        int max = Integer.MIN_VALUE;
        for(int i = 0;i < prices.length;i++){
            int buy = prices[i];
            for(int j = i+1;j < prices.length;j++){
                int sell = prices[j];
                max = Math.max(max, sell - buy);
            }
        }
        return max < 0 ? 0 : max;
    }
}
//双指针
class Solution {
    public int maxProfit(int[] prices) {
        int buy = Integer.MAX_VALUE, profit = Integer.MIN_VALUE;
        
        for(int i = 0;i < prices.length;i++){
            int cur = prices[i];
            buy = Math.min(buy, cur);
            
            profit = Math.max(profit, cur - buy);
        }
        
        return profit;
    }
}
```


### 复杂度

//bf
time: O(N^2)
space: O(1)

//2pointers
time: O(N)
space: O(1)