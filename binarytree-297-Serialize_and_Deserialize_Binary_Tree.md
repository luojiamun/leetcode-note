### 思路

- 拿到题的直觉解法
    - 按照题目本身的例子，给出的是bfs，因此用bfs来序列化和反序列化
    - 需要注意的点是null一定要加进去，保持序列化出来的是满二叉树，这样反序列化就更容易进行

`AC`

- dfs
    - 因为有null的出现，也可以使用dfs来做
    - dfs用preorder，其他应该也可以
    - 难点在递归反序列化的时候，如何处理
        - return: 将这个List<String>反序列化完成后，返回它的root
        - stop: List<String>遇到了null(类似于preorder时遇到了null)
        - params: List<String>

`AC`

### 代码
```java
//bfs
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        Queue<TreeNode> q = new LinkedList<>();
        
        q.add(root);
        
        StringBuilder sb = new StringBuilder();
        
        while(!q.isEmpty()){
            int size = q.size();
            
            for(int i = 0;i < size;i++){
                TreeNode cur = q.poll();
                
                if(cur == null){
                    sb.append("null,");
                } else {
                    sb.append(cur.val);
                    sb.append(",");
                }
                if(cur != null){
                    q.add(cur.left);
                    q.add(cur.right);
                }
                
            }
        }
        String res = sb.toString();
        return res.substring(0, sb.length() - 1);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] nodes = data.split(",");
        
        Queue<TreeNode> q = new LinkedList<>();
        
        for(String node: nodes){
            if(node.equals("null")){
                q.add(null);
            } else {
                TreeNode cur = new TreeNode(Integer.valueOf(node));
                q.add(cur);
            }
            
        }
        Queue<TreeNode> builder = new LinkedList<>();
        TreeNode root = q.poll();
        builder.add(root);
        
        while(!builder.isEmpty()){
            int size = builder.size();
            for(int i = 0;i < size;i++){
                TreeNode cur = builder.poll();
                if(cur != null){
                    cur.left = q.poll();
                    cur.right = q.poll();
                    builder.add(cur.left);
                    builder.add(cur.right);
                }
            }
        }
        return root;
        
    }
}

//dfs
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        String serialization = preorder(root, "");
        return serialization.substring(0, serialization.length() - 1);
    }

    public String preorder(TreeNode root, String serializer){
        if(root == null) {
            serializer += "null,";
            return serializer;
        }
        serializer = serializer + root.val + ",";
        serializer = preorder(root.left, serializer);
        serializer = preorder(root.right, serializer);
        
        return serializer;
    }
    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        List<String> d = new LinkedList<>(Arrays.asList(data.split(",")));
        return preorderD(d);
    }
    
    public TreeNode preorderD(List<String> nodes){
        if(nodes.get(0).equals("null")){
            nodes.remove(0);
            return null;
        }
        
        
        TreeNode cur = new TreeNode(Integer.valueOf(nodes.get(0)));
        nodes.remove(0);
        cur.left = preorderD(nodes);
        cur.right = preorderD(nodes);
        
        return cur;
    }
}
```

### 复杂度

bfs
- serialize
time: 要遍历每个node，O(N)
space: 要维护一个stack，O(N)
- deserialize
time: 要遍历每个node，O(N)
space: 要维护两个stack，O(N)

dfs:
- serialize
time: 要遍历每个node，O(N)
space：递归所需的空间O(H)，H为树的高度，一般为logN，最坏为N。

- deserialize
time: 要遍历每个node，O(N)
space：递归所需的空间O(H)，H为树的高度，一般为logN，最坏为N。需要一个List<String>存储nodes，O(N)。因此总的为O(N).