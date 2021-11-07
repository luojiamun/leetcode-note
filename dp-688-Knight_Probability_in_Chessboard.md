### 思路

- 拿到题的直觉解法
    - dp考察的就是递归函数的推导，能推出来就做出来了大半
    - 这题看了半天没想出思路，参考答案理解了
    - 从起点出发，每次行动都更新一下概率
    - 每次都会从上一次的结果中形成新的可以走到的位置，以及对应的概率

`AC`


### 代码
```java
class Solution {
    public double knightProbability(int n, int k, int row, int column) {
        int[][] directions = new int[][]{{-1, -2}, {-2, -1}, {-2, 1}, {-1, 2}, {1, 2}, {2, 1}, {2, -1}, {1, -2}};
        
        double[][] dp = new double[n][n];
        
        dp[row][column] = 1;
        
        for(int step = 0;step < k;step++){
            double[][] dpt = new double[n][n];
            
            for(int i = 0;i < n;i++){
                for(int j = 0;j < n;j++){
                    for(int[] direction: directions){
                        int nr = i + direction[0];
                        int nc = j + direction[1];
                        if(nr >= 0 && nr < n && nc >= 0 && nc < n){
                            dpt[i][j] += dp[nr][nc] * 0.125;
                        }
                    }
                }
            }
            dp = dpt;
        }
        double res = 0;
        for(int i = 0;i < n;i++){
            for(int j = 0;j < n;j++){
                res += dp[i][j];
            }
        }
        return res;
    }
}
```


### 复杂度

time: O(k*N^2)
space: O(N^2)