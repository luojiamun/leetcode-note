### 思路

- 拿到题的直觉解法
    - 图的题，还是不够熟悉，无论是思路还是写法，还在适应中。
    - 继续沿用island问题中的写法，不需要用directions模版，可以直接用dfs的+1模版，方向相切更容易。

`AC`

### 代码
```java
class Solution {
    public void solve(char[][] board) {
        int n = 0;
        for(int row = 0;row < board.length;row++){
            for(int col = 0;col < board[0].length;col++){
                if((row == 0 || row == board.length - 1 || col == 0 || col == board[0].length - 1) && board[row][col] == 'O') {
                    dfs(board, new int[]{row, col});
                }
            }
        }
        
        for(int row = 0;row < board.length;row++){
            for(int col = 0;col < board[0].length;col++){
                if(board[row][col] == 'A') {
                   board[row][col] = 'O';
                }else if(board[row][col] == 'O') {
                   board[row][col] = 'X';
                }
            }
        }
        
    }
    
    public void dfs(char[][] board, int[] cell){
        if(cell[0] < 0 || cell[0] >= board.length || cell[1] < 0 || cell[1] >= board[0].length || board[cell[0]][cell[1]] != 'O'){
            return;
        }
        board[cell[0]][cell[1]] = 'A';
        
        dfs(board, new int[]{cell[0]+1, cell[1]});
        dfs(board, new int[]{cell[0]-1, cell[1]});
        dfs(board, new int[]{cell[0], cell[1]+1});
        dfs(board, new int[]{cell[0], cell[1]-1});
    }    
}
```

### 复杂度

time: O(N);
space: O(N)