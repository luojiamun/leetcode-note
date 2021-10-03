### 思路

- 拿到题的直觉解法
    - 审题要注意，题目的意思是，如果经过一个node的所有path的pathsum全不符合要求，这个node就不符合要求，要被删除。即，一个node，如果左右两边都不符合要求，它就不符合要求；

`AC`

### 代码
```java
//pre
class Solution {
    public TreeNode sufficientSubset(TreeNode root, int limit) {
        TreeNode dummy = new TreeNode(0);
        dummy.left = root;
        post(dummy, limit, 0);
        return dummy.left;
    }
    
    public boolean preorder(TreeNode root, int limit, int pathsum){
        if(root == null) {
            return false;
        }
        pathsum += root.val;
        if(root.left == null && root.right == null) return pathsum >= limit;
        boolean left = post(root.left, limit, pathsum);
        if(!left) root.left = null;
        boolean right = post(root.right, limit, pathsum);
        if(!right) root.right = null;
        if(!left && !right){
            return false;
        } else {
            return true;
        }
    }
}
```

### 复杂度

time: preorder需要遍历每个node，O(N)
space: 递归所用到的栈，O(logN)，最坏O(N)