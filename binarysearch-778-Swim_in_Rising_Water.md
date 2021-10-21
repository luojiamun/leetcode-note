### 思路

- 拿到题的直觉解法
    - 首先想到dfs，可以做，但是会`TLE`
    - 接下来根据二分，其实可以尝试认为设置上限，看能不能走通；
    - 到这里，就卡住了，因为dfs依然要backtrack，复杂度并没有太多改变；
    - 其实不需要backtrack了，因为我们要求的是`走通`.高上限走得通，就应该降低上线，反之提高；backtrack是怕漏掉了什么，这里似乎不会漏掉什么
    - 如果因为高上限path，导致本轮更优路线即低分值路线没走，那恰好说明这个上限定得太高，要降低；
    - 想通了这一点，就直接dfs，不backtrack，只记录visited
    - 也恰是这一点，优于了直接dfs with backtrack的复杂度。O(2^N)


### 代码
```java
//dfs
class Solution {
    public int swimInWater(int[][] grid) {
        int[][] visited = new int[grid.length][grid[0].length];
        PriorityQueue<Integer> heap = new PriorityQueue<>();
        int res = dfs(grid, 0, 0, visited, Integer.MIN_VALUE, heap).poll();
        return res;
    }
    
    public PriorityQueue<Integer> dfs(int[][] grid, int row, int col, int[][] visited, int pathMax, PriorityQueue<Integer> heap){
        int n = grid.length;
        pathMax = Math.max(pathMax, grid[row][col]);
        if(row == n - 1 && col == n-1){
            heap.add(pathMax);
            return heap;
        }
        if(!heap.isEmpty() && grid[row][col] > heap.peek()) return heap;
        visited[row][col] = 1;
        
        int[][] directions = {{0, 1}, {0, -1}, {-1, 0}, {1, 0}};
        
        for(int[] direction: directions){
            int neighborRow = row + direction[0];
            int neighborCol = col + direction[1];
            
            if(neighborRow >= 0 && neighborRow < n && neighborCol >= 0 && neighborCol < n && visited[neighborRow][neighborCol] == 0){
                dfs(grid, neighborRow, neighborCol, visited, pathMax, heap);
                visited[neighborRow][neighborCol] = 0;
            }
        }
        visited[row][col] = 0;
        return heap;
    }
}

//二分
class Solution {
    public int swimInWater(int[][] grid) {
        
        int n = grid.length;
        
        int left = 0, right = n*n;
        
        while(left <= right){
            int mid = left + (right - left) / 2;
            int[][] visited = new int[grid.length][grid[0].length];
            if(dfs(grid, 0, 0, visited, mid)){
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        int[][] visited = new int[grid.length][grid[0].length];
        if(left > n * n || !dfs(grid, 0, 0, visited, left)){
            return -1;
        }
        return left;
    }
    
    public boolean dfs(int[][] grid, int row, int col, int[][] visited, int mid){
        int n = grid.length;
        if(row < 0 || row >= grid.length || col < 0 || col >= grid.length || visited[row][col] == 1 || grid[row][col] > mid){
            return false;
        }
        if(row == n - 1 && col == n - 1) return true;
        
        visited[row][col] = 1;
        return dfs(grid, row+1, col, visited, mid) || dfs(grid, row-1, col, visited, mid) || dfs(grid, row, col+1, visited, mid) || dfs(grid, row, col-1, visited, mid);
    }
}
```


### 复杂度

//dfs with backtrack
time: O(2^N)
space: O(N)

//dfs with binarysearch
time: O(NlogMax), Max = Math.max(nums)
space: O(N)