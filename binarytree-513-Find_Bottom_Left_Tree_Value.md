### 思路

- 拿到题的直觉解法
    - 找最后一行，也就是距离root最远的一层，直觉用bfs
    - 建一个List<List<TreeNode>> 存储每一层的元素list
    - 建一个q存储每一层的元素

`AC`

- dfs
    - 可以用preorder
    - 递归设置
        - return: 当前树最深的node其值和深度，存为int[2], 设定为[node.val, depth]
        - stop: node == null返回[0,0](深度为0会被淘汰)
        - params: root，及其深度信息depth

`AC`

### 代码
```java
//bfs
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        if(root == null){
            return Integer.MIN_VALUE;
        }
        
        List<List<TreeNode>> levels = new ArrayList<>();
        
        Queue<TreeNode> q = new LinkedList<>();
        
        q.add(root);
        
        while(!q.isEmpty()){
            int size = q.size();
            List<TreeNode> level = new ArrayList<>();
            for(int i = 0;i < size;i++){
                TreeNode cur = q.poll();
                level.add(cur);
                if(cur.left != null) q.add(cur.left);
                if(cur.right != null) q.add(cur.right);
            }
            levels.add(level);
        }
        
        return levels.get(levels.size() - 1).get(0).val;
    }
}

//dfs
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        return preorder(root, 1)[0];
    }
    
    public int[] preorder(TreeNode root, int depth){
        if(root == null) return new int[2];
        
        if(root.left == null && root.right == null) return new int[]{root.val, depth};
        
        depth++;
        int[] left = preorder(root.left, depth);
        int[] right = preorder(root.right, depth);
        
        if(left[1] >= right[1]){
            return left;
        } else {
            return right;
        }
    }
}
```

### 复杂度

bfs
time: 要遍历每个node，O(N)
space: 要维护一个q，O(N),要维护一个List<List<TreeNode>>，O(N),总的来说O(N);

dfs:
time: 要遍历每个node，O(N)
space：递归所需的空间O(H)，H为树的高度，一般为logN，最坏为N。