### 思路

- 拿到题的直觉解法
    - 做过了，知道怎么做，所以相当于可以背出答案；
    - 但是对于这个问题的抽象，没有内化，所以稍微一变形可能就不会；
    - 目前只理解到这个求解的过程，top-down来看，是因为每次遇到当前值不等，其实就分出了一个新的路径或者dp，当前值由新路径的最优解得到；
    - 对于这个题来说，最优解就是最长子序列；这就是为什么会出现max的原因；
    - 而因为路径是有限的，其实就两条，因为只有两个str，所以很容易就能在二维数组给实现了；但这种抽象是不够的。要继续多练。

`AC`


### 代码
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        
        int[][] dp = new int[m+1][n+1];
        
        for(int row = m - 1;row >=0;row--){
            for(int col = n - 1;col >= 0;col--){
                if(text1.charAt(row) == text2.charAt(col)){
                    dp[row][col] = dp[row+1][col+1] + 1;
                } else {
                    dp[row][col] = Math.max(dp[row+1][col], dp[row][col+1]);
                }
            }
        }
        return dp[0][0];
    }
}
```


### 复杂度

time: O(m*n)
space: O(m*n)