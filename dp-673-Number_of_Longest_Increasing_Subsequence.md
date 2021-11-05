### 思路

- 拿到题的直觉解法
    - 在看LIS专题，没思路，先打卡；
    - 应该是可以用递归和递推都来一遍，之后更新；

`AC`


### 代码
```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        int n = nums.length;
        int[][] dp = new int[n][2];
        for(int[] row: dp){
            Arrays.fill(row, 1);
        }
                
        int max = 1;
        
        for(int i = 0;i < n;i++){
            for(int j = i+1;j < n;j++){
                if(nums[j] > nums[i]){
                    if(dp[i][0] + 1 > dp[j][0]){
                        dp[j][0] = dp[i][0] + 1;
                        dp[j][1] = dp[i][1];
                        max = Math.max(max, dp[j][0]);
                    } else if(dp[i][0] + 1 == dp[j][0]){
                        dp[j][1] += dp[i][1];
                    }
                }
                System.out.println(dp[j][0] +":"+dp[j][1]);

            }
        }
        
        int sum = 0;
        
        for(int i = 0;i < n;i++){
            if(dp[i][0] == max){
                sum += dp[i][1];
            }
        }
        
        return sum;
    }
}
```


### 复杂度

time: O(N^2)
space: O(N)