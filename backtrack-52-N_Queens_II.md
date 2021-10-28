### 思路

- 拿到题的直觉解法
    - 写过的，经典题；然而还是忘了那个对角线trick...
    - 可以暴力解，但写起来很复杂。
    - 总的来说，当你知道了对角线的trick之后，就是标准的backtrack，唯一需要的简化写法的就是，你没必要遍历row了，用递归就是row了。

`AC`

### 代码
```java
//backtrack
class Solution {
    public int totalNQueens(int n) {
        HashSet<Integer> cols = new HashSet<>();
        HashSet<Integer> ldiags = new HashSet<>();
        HashSet<Integer> rdiags = new HashSet<>();
        return backtrack(n, 0, -100, cols,ldiags,rdiags, 0);
        
        
    }
    
    public int backtrack(int n, int row, int pre, HashSet<Integer> cols,HashSet<Integer> ldiags, HashSet<Integer> rdiags,int res){
        if(row == n){
            return ++res;
        }
        
        for(int col = 0;col < n;col++){
            int lDiag = row + col;
            int rDiag = row - col;
            if(!cols.contains(col) && !ldiags.contains(lDiag) && !rdiags.contains(rDiag) && pre + 1 != col && pre - 1 != col){
                cols.add(col);
                ldiags.add(row + col);
                rdiags.add(row - col);
                res = backtrack(n, row+1, col, cols,ldiags,rdiags, res);
                cols.remove(col);
                ldiags.remove(row + col);
                rdiags.remove(row - col);
            }
        }
        return res;
    }
}
```


### 复杂度

time: O(N!)，排列组合，第一次N个选择，第二次N-1...
space: O(N)，递归了N层，代表有N行；Set也需要用到N