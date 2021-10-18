### 思路

- 拿到题的直觉解法
    - 看错了题意，以为是要同根的，原来刚好相反；
    - 花了不少时间，在想到底返回什么，boolean,int？其实都不合适
    - 应该返回一个list存储两个值的深度，最后比较一下

`AC`


### 代码
```java
//寻找最右插入位置
class Solution {
    public boolean isCousins(TreeNode root, int x, int y) {
        List<Integer> res = new ArrayList<>();
        
        preOrder(root, x, y, res, 0);
        return res.size() == 2 && res.get(0) == res.get(1);
    }
    
    public List<Integer> preOrder(TreeNode root, int x, int y, List<Integer> res, int depth){
        if(root == null || res.size() == 2) return res;
        if(root.val == x || root.val == y) {
            res.add(depth);
            return res;
        }
        if(root.left != null && root.right != null && ((root.left.val == x && root.right.val == y) || ((root.left.val == y && root.right.val == x)))){
            res.add(1);
            res.add(2);
            return res;
        }
        preOrder(root.left, x, y, res, depth+1);
        preOrder(root.right, x, y, res, depth+1);
        
        return res;
    }
}
```


### 复杂度

time: ON),遍历N个node
space: O(1)