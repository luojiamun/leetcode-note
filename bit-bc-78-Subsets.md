### 思路

- 拿到题的直觉解法
    - backtrack可以直接做；
    - bit这个思路真是很神奇；
    - 性质：利用1 << i可以挪位，利用&可以比较当前位置和目标是否匹配都为1；
    - 性质：利用每个元素的选与不选两种状态，2^N即为所有可能性


`AC`


### 代码
```java
//回溯
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        for(int i = 0;i <= nums.length;i++){
            List<Integer> subset = new ArrayList<>();
            backtrack(nums, i, subset, 0);
        }
        return res;
    }
    
    public boolean backtrack(int[] nums, int size, List<Integer> subset, int start){
        if(subset.size() == size){
            res.add(new ArrayList(subset));
            return true;
        }
        
        for(int i = start;i < nums.length;i++){
            subset.add(nums[i]);
            backtrack(nums, size, subset, i+1);
            subset.remove(subset.size() - 1);
        }
        return false;
    }
}

//bit操作很神奇
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        
        int start = 0, end = (int)Math.pow(2, nums.length);
        
        for(int sign = start;sign < end;sign++){
            List<Integer> set = new ArrayList<>();
            
            for(int i = 0;i < nums.length;i++){
                if(((1 << i) & sign) != 0){
                    set.add(nums[i]);
                }
            }
            res.add(set);
        }
        
        return res;
    }
}
```


### 复杂度

time: O(N * 2^N)
space: O(N)