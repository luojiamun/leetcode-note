### 思路

- 拿到题的直觉解法
    - 很常规的题，写了好多次，正确率很低；
    - 老问题，心里没模版，每次都是抓瞎，碰运气；
    - 图的题看起来就是dfs/bfs，特别是像这种模版题，自查的点主要是写法
    - 这题可以和994放在一起看
    - 这两题的卡点在于，要注意`蔓延`这个行为，是可以从任意陆地/烂番茄的地方开始的，因此要么你逐个bfs然后更新最少时间，要么就模拟一起开始
    - 技巧也就是在模拟一起开始，因为是同时开始，全部一次性放入q，那么每次更新到的邻居肯定都是最短时间的了，这就省去了时间更新这一步；同时也使得流程写起来更简洁

`AC`


### 代码
```java
class Solution {
    public int maxDistance(int[][] grid) {
        int max = Integer.MIN_VALUE;
        
        Queue<int[]> q = new LinkedList<>();
        
        for(int row = 0;row < grid.length;row++){
            for(int col = 0;col < grid[0].length;col++){
                if(grid[row][col] == 1){
                    q.add(new int[]{row, col});
                }
            }
        }
        
        if(q.size() == grid.length * grid[0].length || q.size() == 0){
            return -1;
        }
        
        int[][] directions = new int[][]{{-1, 0}, {0, 1}, {1, 0}, {0, -1}};
        
        while(!q.isEmpty()){
            int size = q.size();
            
            for(int i = 0;i < size;i++){
                int[] cur = q.poll();
                int dis = grid[cur[0]][cur[1]] + 1;
                
                for(int[] direction: directions){
                    int nrow = cur[0] + direction[0];
                    int ncol = cur[1] + direction[1];
                    if(nrow >= 0 && nrow < grid.length && ncol >= 0 && ncol < grid[0].length && grid[nrow][ncol] == 0){
                        grid[nrow][ncol] = dis;
                        q.add(new int[]{nrow, ncol});
                    }
                }
            }
        }
        
        for(int row = 0;row < grid.length;row++){
            for(int col = 0;col < grid[0].length;col++){
                max = Math.max(max, grid[row][col]);
            }
        }
        
        return max - 1;
            
    }
}
```


### 复杂度

time: O(N^2)，遍历每个cell
space: O(N)
