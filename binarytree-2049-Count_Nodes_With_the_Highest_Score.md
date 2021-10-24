### 思路

- 拿到题的直觉解法
    - 比赛时又犯了提前优化的老毛病
    - 直觉上，这是个树的题，应该建个树，然后通过遍历来解决；
    - 比赛时走偏了，希望不借助树直接解决。理论上是可行的，比赛时也尝试了，没成功，结束后继续尝试了，TLE.
    - 其实直接建树，通过遍历可以解决，虽然写的依旧繁琐，但是可以过


`AC`


### 代码
```java
class Solution {
    HashMap<Node, Integer> cache =new HashMap<>();
    public int countHighestScoreNodes(int[] parents) {
        int n = parents.length;
        Node[] nodes = new Node[n];
        HashMap<Long, Integer> res = new HashMap<>();
        Long max = Long.MIN_VALUE;
        for(int i = 0;i < n;i++){
            nodes[i] = new Node(i);
        }
        
        for(int i = 1;i < n;i++){
            Node node = nodes[i];
            Node parent = nodes[parents[i]];
            if(parent.left == null){
                parent.left = node;
            } else {
                parent.right = node;
            }
        }
        
        for(int i = 0;i < n;i++){
            Node toBeRemoved = nodes[i];
            int leftCount = preorder(toBeRemoved.left);
            int rightCount = preorder(toBeRemoved.right);
            int others = n - leftCount - rightCount - 1;
            leftCount = leftCount > 0?leftCount:1;
            rightCount = rightCount > 0?rightCount:1;
            others = others > 0?others:1;
            Long product = (long)leftCount * rightCount * others;
            res.putIfAbsent(product, 0);
            res.put(product, res.get(product)+1);
            max = Math.max(max, product);
        }
        
        return res.get(max);
    }
    
    public int preorder(Node root){
        if(root == null) return 0;
        if(cache.containsKey(root)) return cache.get(root);
        
        int left = preorder(root.left);
        if(root.left != null) cache.put(root.left, left);
        int right = preorder(root.right);
        if(root.right != null) cache.put(root.right, right);
        
        return 1+left+right;
    }
    
    static class Node{
        Node left;
        Node right;
        int val;
        public Node(int val){
            this.val = val;
        }
    }
}
```


### 复杂度

time: O(N)
space: O(N)