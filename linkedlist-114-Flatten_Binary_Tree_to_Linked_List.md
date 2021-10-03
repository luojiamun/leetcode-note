### 思路

- 拿到题的直觉解法
    - 递归，先收集nodes再拼接；
    - 递归，不收集nodes直接拼接；

`AC`

### 代码
```java
class Solution {
    public void flatten(TreeNode root) {
        preorder(root);
        return;
    }
    
    public TreeNode preorder(TreeNode root){
        if(root == null) return null;
        TreeNode tempL = root.left;
        TreeNode tempR = root.right;
        root.left = null;
        root.right = preorder(tempL);
        TreeNode cur = root;
        while(cur.right != null){
            cur = cur.right;
        }
        cur.right = preorder(tempR);
        return root;
    }
}

class Solution {
    public void flatten(TreeNode root) {
        List<TreeNode> nodes = new ArrayList<>();
        
        List<TreeNode> res = preorder(root, nodes);
        
        for(int i = 0;i < res.size()-1;i++){
            TreeNode cur = res.get(i);
            TreeNode next = res.get(i+1);
            
            cur.left = null;
            cur.right = next;
        }
        return;
    }
    
    public List<TreeNode> preorder(TreeNode root, List<TreeNode> res){
        if(root == null) return res;
        
        res.add(root);
        preorder(root.left, res);
        preorder(root.right, res);
        return res;
    }
}
```

### 复杂度

time: O(N);
space：map所需的空间O(N)