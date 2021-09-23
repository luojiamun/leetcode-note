### 思路

- 拿到题的直觉解法
    - 树的问题都是遍历问题；
    - 求路径问题，顺带求路径上的值的和；
    - 可以把路径作为参数随函数带着，很方便；
    - 节点的值，需要知道根的值作为基础，因此是个pre-order
    - 题目要返回的总和，可以通过global，也可以通过return
        - global：total；递归为void，参数带了当前node之前的pathsum信息。更新当前node的score后往下传递，直到遇到左右都为null的叶子结点，更新total；
        - return：递归为total；遇到左右都为null的叶子结点，返回score；

`AC`

### 代码
```java
//use global to save total
class Solution {
    int total;
    public int sumNumbers(TreeNode root) {
        preorder(root, 0);
        return total;
    }
    /*
        return: void；在递归过程中更新total；
        stop: root == null or leaf
        param: root, pathSum (before root)
    */
    public void preorder(TreeNode root, int pathscore){
        if(root == null){
            return;
        }
        
        int curPathScore = pathscore * 10 + root.val;
        
        if(root.left == null && root.right == null){
            total += curPathScore;
            return;
        }
        preorder(root.left, curPathScore);
        preorder(root.right, curPathScore);
    }
}

//use recusion return as total
class Solution {
    public int sumNumbers(TreeNode root) {
        return preorder(root, 0);
    }
    
    /*
        return: sum of leaf score which is pathsum, in this branch;
        stop: root == null or leaf
        param: root, pathSum (before root)
    */
    public int preorder(TreeNode root, int prepathSum){
        if(root == null) return 0;
        
        int curPathSum = prepathSum * 10 + root.val;
        
        if(root.left == null && root.right == null) return curPathSum;
        
        int left = preorder(root.left, curPathSum);
        int right = preorder(root.right, curPathSum);
        
        return left+right;
    }
}
```

### 复杂度

time: 每个节点都遍历到了，因此为O(N)。注意与递归无关。
space: 递归所需要的stack，要求O(logN)，亦即tree的高度;