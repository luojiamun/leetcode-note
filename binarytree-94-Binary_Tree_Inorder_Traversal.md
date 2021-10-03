### 思路

- 拿到题的直觉解法
    - 尝试不适用global var来存储结果
    - 递归
        - return：这个树所有node的val的集合；
        - stop: root == null
        - param: root, path (vals of nodes的集合)

`AC`

- bfs

`AC`

### 代码
```java
//bfs
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root == null){
            List<Integer> res = new ArrayList<>();
            return res;
        }
        
        int WHITE = 0, GRAY = 1;
        
        Stack<Tuple> stack = new Stack<>();
        
        stack.add(new Tuple(WHITE, root));
        
        List<Integer> res = new ArrayList<>();
        
        while(!stack.isEmpty()){
            Tuple cur = stack.pop();
            
            if(cur.color == GRAY){
                res.add(cur.node.val);
            } else {
                if(cur.node.right != null) stack.add(new Tuple(WHITE, cur.node.right));
                stack.add(new Tuple(GRAY, cur.node));
                if(cur.node.left != null) stack.add(new Tuple(WHITE, cur.node.left));
            }
        }
        
        return res;
    }
    
    public class Tuple{
        int color;
        TreeNode node;
        
        public Tuple(int color, TreeNode node){
            this.color = color;
            this.node = node;
        }
    }
}

//dfs
/*
return: all vals from nodes of this tree;
stop: root == null
param: root, all vals from nodes pre of this node

*/
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        
        inorder(root, res);
        
        return res;
    }
    
    public List<Integer> inorder(TreeNode root, List<Integer> path) {
        if(root == null) return path;
        
        List<Integer> left = inorder(root.left, path);
        path.add(root.val);
        List<Integer> right = inorder(root.right, path);
        
        return path;
    }
}
```

### 复杂度

bfs
time: 要遍历每个node，O(N)
space: 要维护一个stack，O(N)

dfs:
time: 要遍历每个node，O(N)
space：递归所需的空间O(H)，H为树的高度，一般为logN，最坏为N。要维护一个List<Integer>,O(N);总的来看O(N).