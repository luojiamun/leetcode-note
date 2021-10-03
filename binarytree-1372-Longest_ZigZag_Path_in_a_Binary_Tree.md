### 思路

- 拿到题的直觉解法
    - 这个题思路不难，但写起来很容易写错。
    - 一定要思路很清楚，对每个参数的设定的含义，一定要搞清楚，不然的话，就会有逻辑错误，能通过一部分测试，却不能通过另一部分；

`AC`

### 代码
```java
class Solution {
    public int longestZigZag(TreeNode root) {
        if(root.left == null && root.right == null) return 0;
        int l = preorder(root.left, 1, 1, true);
        int r = preorder(root.right, 1, 1, false);
        return Math.max(l, r);
    }
    
    public int preorder(TreeNode root, int pathsum, int max, boolean left){
        if(root == null){
            return max;
        }        
        max = Math.max(max, pathsum);
        int l, r;
        if(left){
            l = preorder(root.left, 1, max, true);
            r = preorder(root.right, pathsum+1, max, false);
        } else {
            l = preorder(root.left, pathsum+1, max, true);
            r = preorder(root.right, 1, max, false);
        }
        
        return Math.max(l, r);
    }
}
```

### 复杂度

time: preorder需要遍历每个node，O(N)
space: 递归所用到的栈，O(logN)，最坏O(N)