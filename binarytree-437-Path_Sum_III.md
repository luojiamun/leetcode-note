### 思路

- 拿到题的直觉解法
    - 只想到了将所有的path找出来，然后再去找符合条件的subarray
    - 即想到了暴力解，但也没实现；

`Failed`

- 前缀和，2sum
- 边preorder，边得到前缀和
- 每次都做一次前缀和2sum


### 代码
```java
class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        HashMap<Integer, Integer> map = new HashMap<>();
        
        return preorder(root, targetSum, map, 0); 
    }
    
    public int preorder(TreeNode root, int targetSum, HashMap<Integer, Integer> map, int presum){
        if(root == null) return 0;
        presum += root.val;
        int count = 0;
        if(presum == targetSum) count++;
        
        if(map.containsKey(presum - targetSum)){
            count += map.get(presum - targetSum);
        }
        map.put(presum, map.getOrDefault(presum, 0) + 1);
        
        int l = preorder(root.left, targetSum, map, presum);
        int r = preorder(root.right, targetSum, map, presum);
        
        map.put(presum, map.get(presum) - 1);
        return count+l+r;
        
    }
}
```


### 复杂度

time: O(N)
space: O(N)