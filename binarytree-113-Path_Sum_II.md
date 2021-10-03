### 思路

- 拿到题的直觉解法
    - 用preorder，将所有符合条件的都收集到一个list
    - 递归过程
        - return: 所有符合条件的路径
        - stop: root == null
        - param: 我们不用global，只用return和参数；因此需要带上root，路径path，路径和pathsum用于判断；targetSum是判断条件，res是作为符合条件的收集器

`AC`

### 代码
```java
//pre
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        return preorder(root, new ArrayList<>(), 0, new ArrayList<>(), targetSum);
    }
    
    public List<List<Integer>> preorder(TreeNode root, List<Integer> path, int pathsum, List<List<Integer>> res, int targetSum){
        if(root == null) return res;
        
        path.add(root.val);
        pathsum += root.val;
        
        if(root.left == null && root.right == null && pathsum == targetSum){
            res.add(new ArrayList<>(path));
        }
        
        preorder(root.left, new ArrayList<>(path), pathsum, res, targetSum);
        preorder(root.right, new ArrayList<>(path), pathsum, res, targetSum);
        
        return res;
    }
}
```

### 复杂度

time: preorder需要遍历每个node，O(N)
space: 递归所用到的栈，O(logN)，最坏O(N)