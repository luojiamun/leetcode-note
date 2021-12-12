### 思路

- 拿到题的直觉解法
    - 从起点到终点，是个递归关系，因为只有向下和向右，因此dfs的每个分支只要能到终点都是distinct的。注意记忆化；
    - 从终点到起点，终点由其左边和上边的情况决定，依此类推，那么可以从起点，递推到终点；

`AC`


### 代码
```java
//递归，记忆化
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] map = new int[m][n];
        for(int[] row: map) Arrays.fill(row, -1);
        return dfs(0, 0, m, n, map);
    }
    public int dfs(int row, int col, int m, int n, int[][] map){
        if(row >= m || col >= n) return 0;
        
        if(row == m - 1 && col == n - 1) return 1;
        
        if(map[row][col] != -1) return map[row][col];
        
        int down = dfs(row+1, col, m, n, map);
        
        int right = dfs(row, col + 1, m, n, map);
        
        map[row][col] = down + right;
        
        return map[row][col];
    }
}

//递推，dp
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] map = new int[m][n];
        map[0][0] = 1;
        for(int row = 0;row < m;row++){
            for(int col = 0;col < n;col++){
                if(row == 0 && col == 0) continue;
                int fromLeft = col - 1 >= 0?map[row][col - 1]:0;
                int fromUp = row - 1 >= 0?map[row - 1][col]:0;
                
                map[row][col] = fromLeft + fromUp;
            }
        }
        
        return map[m - 1][n - 1];
    }
}
```


### 复杂度

time: O(m*n)
space: O(m*n)