### 思路

- 拿到题的直觉解法
    - 树的问题都是有关遍历的；
    - 既然是比较Tree，那么就相当于同步遍历；
    - 在同步过程中一旦发现不同马上返回false；

- 递归
    - return：以p和q为root的两树是否相同(drill down之后即：子树是否相同)
    - stop: 如果两树同时到达null，或者有一个树到达null
    - param: 两树的root

`AC`

### 代码
```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        return preorder(p, q);
    }
    
    public boolean preorder(TreeNode p, TreeNode q) {
        if(p == null && q == null) return true;
        if(p == null || q == null) return false;
        
        
        return p.val == q.val && preorder(p.left, q.left) && preorder(p.right, q.right);
    }
}
```

### 复杂度

dfs
time: 最坏情况，每个node都比较到了，因此O(N)
space: 维护递归stack的空间，由递归的层数决定，因此为O(logN);