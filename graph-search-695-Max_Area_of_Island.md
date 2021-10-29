### 思路

- 拿到题的直觉解法
    - 标准的graph dfs/bfs模版题；
    - 难点不在思路，而是在写法上；
    - 这题做过很多遍，每次写法都不同，但都很丑很差很啰嗦；这种问题就是写完了没思考没整理模版造成的。
    - 第一次dfs：其实没必要directions，也没必要visited，返回值size在递归参与运算也有点危险；总的来说就是只为这个题；
    - 第二次bfs：也很啰嗦；
    - 第三次dfs：递归更清晰(递归返回值：1即当前cell面积+分支的面积和，结束条件越界或非1)，visited就是在线改；这种写法写多几次，就会有印象什么时候合适。

`AC`


### 代码
```java
//第一次dfs
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int[][] visited = new int[grid.length][grid[0].length];
        int[][] directions = new int[][]{{-1,0},{0,1},{1,0},{0,-1}};
        int max = 0;
        for(int row = 0;row < grid.length;row++){
            for(int col = 0;col < grid[0].length;col++){
                if(grid[row][col] != 0 && visited[row][col] != 1){
                    max = Math.max(max, dfs(grid, new int[]{row, col}, 0, visited, directions));
                }
            }
        }
        return max;
    }
    
    public int dfs(int[][] grid, int[] cur, int size, int[][] visited, int[][] directions){
        
        size++;
        visited[cur[0]][cur[1]] = 1;
        for(int[] direction: directions){
            int nextRow = cur[0] + direction[0];
            int nextCol = cur[1] + direction[1];
            if(nextRow >= 0 && nextRow < grid.length && nextCol >= 0 && nextCol < grid[0].length && visited[nextRow][nextCol] == 0 && grid[nextRow][nextCol] != 0){
                visited[nextRow][nextCol] = 1;
                size = dfs(grid, new int[]{nextRow, nextCol}, size, visited, directions);
            } 
        }
        
        return size;
        
        
    }
}
//第二次bfs
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int res = 0;
        for(int row = 0;row < grid.length;row++){
            for(int col = 0;col < grid[0].length;col++){
                if(grid[row][col] == 1){
                    res = Math.max(res, bfs(grid, row, col));
                }
            }
        }
        
        return res;
    }
    
    public int bfs(int[][] grid, int row, int col){
        int res = 0;
        int[][] directions = {{1,0},{0,1},{-1,0},{0,-1}};
        Queue<int[]> q = new LinkedList<>();
        
        q.add(new int[]{row, col});
        res++;
        grid[row][col] = 0;
        while(!q.isEmpty()){
            int size = q.size();
            
            for(int i = 0;i < size;i++){
                int[] cur = q.poll();
                for(int[] direction: directions){
                    int nr = cur[0] + direction[0];
                    int nc = cur[1] + direction[1];
                    if(nr >= 0 && nr < grid.length && nc >= 0 && nc < grid[0].length && grid[nr][nc] == 1){
                        q.add(new int[]{nr, nc});
                        res++;
                        grid[nr][nc] = 0;
                    }
                }
                
            }
        }
        return res;
    }
}
//第三次
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int max = 0;
        for(int row = 0;row < grid.length;row++){
            for(int col = 0;col < grid[0].length;col++){
                if(grid[row][col] == 1){
                    max = Math.max(max, dfs(grid, new int[]{row, col}));
                }
            }
        }
        
        return max;
    }
    
    public int dfs(int[][] grid, int[] cell){
        if(cell[0] < 0 || cell[0] >= grid.length || cell[1] < 0 || cell[1] >= grid[0].length || grid[cell[0]][cell[1]] == 0){
            return 0;
        }
        
        grid[cell[0]][cell[1]] = 0;
        return 1 + dfs(grid, new int[]{cell[0] + 1, cell[1]}) + dfs(grid, new int[]{cell[0] - 1, cell[1]}) + dfs(grid, new int[]{cell[0], cell[1] + 1}) + dfs(grid, new int[]{cell[0], cell[1] - 1});
    }
}
```


### 复杂度
N = row * col
time: O(N)，遍历每个cell
space: O(N)，最坏情况满格的话每个cell都drill down形成了N层stack
