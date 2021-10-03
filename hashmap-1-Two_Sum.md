### 思路

- 拿到题的直觉解法
    - use hashmap

`AC`

### 代码
```java
//use hashmap
class Solution {
    public int[] twoSum(int[] nums, int target) {
        //create map to save [visited, index]
        HashMap<Integer, Integer> map = new HashMap<>();
        //traverse nums
        for(int i = 0;i < nums.length;i++){
            if(map.containsKey(target - nums[i])){
                return new int[]{map.get(target - nums[i]), i};
            } else{
                map.put(nums[i], i);
            }
        }
        return new int[2];
    }
}
```

### 复杂度

//use hashmap
time: 需要遍历nums，O(N)
space: 需要hashmap，O(N)