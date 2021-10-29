### 思路

- 拿到题的直觉解法
    - 很常规的题，写了好多次，正确率很低；
    - 老问题，心里没模版，每次都是抓瞎，碰运气；
    - 这题的卡点在于，要注意rotting可以同时在所有的烂番茄处开始，因此要么你逐个bfs然后更新最少时间，要么就模拟一起开始
    - 技巧也就是在模拟一起开始，因为是同时开始，全部一次性放入q，那么每次更新到的邻居肯定都是最短时间的了，这就省去了时间更新这一步；

`AC`


### 代码
```java
class Solution {
    public int orangesRotting(int[][] grid) {
        int res = 0;
        Queue<int[]> q = new LinkedList<>();
        int good = 0;
        int[][] directions = new int[][]{{0,1},{1,0},{0,-1},{-1,0}};
       
        for(int row = 0;row < grid.length;row++){
            for(int col = 0;col < grid[0].length;col++){
                if(grid[row][col] == 1){
                    good++;
                } else if(grid[row][col] == 2){
                    q.add(new int[]{row, col});
                }
            }
        }
        if(good == 0) return 0;
        while(!q.isEmpty()){
            int size = q.size();
            res++;
            for(int i = 0;i < size;i++){
                int[] cur = q.poll();
                for(int[] direction: directions){
                    int nrow = cur[0] + direction[0];
                    int ncol = cur[1] + direction[1];
                    
                    if(nrow >= 0 && nrow < grid.length && ncol >= 0 && ncol < grid[0].length && grid[nrow][ncol] == 1){
                        grid[nrow][ncol] = 2;
                        good--;
                        q.add(new int[]{nrow, ncol});
                    }
                }
            }
        }
        return good == 0?res-1:-1;
    }
}

//需要更新时间，繁琐
class Solution {
    public int orangesRotting(int[][] grid) {
        //find the rotten ones, make 0->-1, 1->Integer.MAX_VALUE, 2->0;
        List<int[]> rottens = new ArrayList<>();
        for(int row = 0;row < grid.length;row++){
            for(int col = 0;col < grid[0].length;col++){
                if(grid[row][col] == 2){
                    grid[row][col] = 0;
                    rottens.add(new int[]{row, col});
                } else if(grid[row][col] == 1){
                    grid[row][col] = Integer.MAX_VALUE;
                } else {
                    grid[row][col] = -1;
                }
            }
        }
        
        //setup directions
        int[][] directions = new int[][]{{0, -1}, {-1, 0}, {0, 1}, {1, 0}};//a, w, d, s
        
        //iterate rotten ones with bfs each, update with min time
        for(int[] rotten: rottens){
            Queue<int[]> q = new LinkedList<>();
            q.add(rotten);
            int time = 0;
            while(!q.isEmpty()){
                int size = q.size();//size of current level
                
                for(int i = 0;i < size;i++){
                    int[] cur = q.poll();
                    // System.out.println(grid[cur[0]][cur[1]]);
                    if(grid[cur[0]][cur[1]] == -1) continue;
                    if(grid[cur[0]][cur[1]] > time) grid[cur[0]][cur[1]] = time;
                    
                    for(int[] direction: directions){
                        int neighbor_r = cur[0] + direction[0];
                        int neighbor_c = cur[1] + direction[1];
                        // System.out.println(neighbor_r + " " + neighbor_c);
                        if(neighbor_r >= 0 && neighbor_r < grid.length && neighbor_c >= 0 && neighbor_c < grid[0].length && grid[cur[0]][cur[1]] < grid[neighbor_r][neighbor_c]){
                            q.add(new int[]{neighbor_r, neighbor_c});
                        }
                    }
                }
                time++;
            }
        }
        
        //find the max value on the map
        int max_time = 0;
        for(int row = 0;row < grid.length;row++){
            for(int col = 0;col < grid[0].length;col++){
                // System.out.println(grid[row][col]);
                if(grid[row][col] == Integer.MAX_VALUE) return -1;
                if(grid[row][col] > 0) max_time = Math.max(max_time, grid[row][col]);
            }
        }
        
        return max_time;
        
        
    }
}
```


### 复杂度
time: O(N)，遍历每个cell
space: O(N)，最坏情况满格的话每个cell都drill down形成了N层stack

更新时间的方法
time: O(N^2)因为要重复计算
space: O(N)