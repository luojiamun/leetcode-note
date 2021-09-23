### 思路

- 拿到题的直觉解法
    - 求深度，即层数，直接想到了bfs；
    - 建一个q存储每层的node
    - 按照每层一个list的思路，放到q
    - while循环，每次处理一层
    *上面的做法建立list是多余的，每次处理一层，那么每次都处理q内所有元素即为当前层，无需list*
    - 将每层node直接放入q
    - 每个while循环处理一层node

`AC`

- 接下来思考dfs递归解法
    - 递归要形成一个思维习惯，有几个步骤
        - return: 当前分枝的最大深度；
        - stop: 当前分枝为null
        - param: root, depth
    - 然后将递归作为一个黑盒api
        - 输入分枝和当前位置深度+1；
        - 返回该分枝最大深度；
    - 将递归函数作为正常函数在逻辑中调用；

`AC`


### 代码
```java
//bfs
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        
        Queue<TreeNode> q = new LinkedList<>();
        
        q.add(root);
        
        int depth = 0;
        
        while(!q.isEmpty()){
            depth++;
            int size = q.size();
            for(int i = 0;i < size;i++){
                TreeNode node = q.poll();
                if(node.left != null) q.add(node.left);
                if(node.right != null) q.add(node.right);
            }
        }
        
        return depth;
    }
}

//dfs, 可以简化为几行剪短代码，选择保留以看得清结构
class Solution {
    public int maxDepth(TreeNode root) {
        return dfs(root, 1);
    }
    
    /*
        param: root, depth
        stop: root == null
        return: depth
    */
    
    public int dfs(TreeNode root, int depth){
        if(root == null) return depth - 1;
        
        int left = dfs(root.left, depth+1);
        int right = dfs(root.right, depth+1);
        
        return Math.max(left, right);
    }
}
```

### 复杂度

bfs
time: 要遍历每一个node，因此时间复杂度O(N)
space: 需要维护一个q，这个q的大小由每一层的最多的node个数决定。
2^0 + 2^1 + 2^2 + ... + 2^k = N, where k+1是层数。
根据求和公式，(2^(k+1) - 1)/(2 - 1) = N, 
因此k = logN  - 1，于是最后一层元素最多可能有N/2个node。
因此维护这个q需要O(N)的空间。

dfs
time: 虽然递归只进行了logN次，但是每个node依旧被遍历到了，因此时间复杂度还是O(N)
space: 维护递归stack的空间，由递归的层数决定，因此为O(logN);