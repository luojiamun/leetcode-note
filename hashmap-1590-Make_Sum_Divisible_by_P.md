### 思路

即lc 1590，用的lc题目条件写的。

- 拿到题的直觉解法
    - 暴力解法，利用前缀和，将所有可能的subarray都检查一遍；

`TLE`

- 利用前缀和，以及余数的规律；

`AC`

### 代码
```java
//hashmap, presum, mod
class Solution {
    public int minSubarray(int[] nums, int p) {
        long sum = 0;
        
        for(int i: nums){
            sum += i;
        }
        if(sum < p) return -1;
        
        int mod = (int)(sum % p);
        if(mod == 0) return 0;
        
        HashMap<Integer, Integer> modMap = new HashMap<>();
        modMap.put(0, -1);
        long prefix = 0;
        int min = nums.length;
        for(int i = 0;i < nums.length;i++){
            prefix += nums[i];
            modMap.put((int)(prefix % p),i);
            if(modMap.containsKey((int)((prefix-mod) % p))){
                min = Math.min(min, i-modMap.get((int)((prefix-mod) % p)));
            }
        }
        return min == nums.length?-1:min;
    }
}

//presum brutal force, TLE
class Solution {
    public int minSubarray(int[] nums, int p) {
        long[] preSum = new long[nums.length];
        preSum[0] = (long)nums[0];
        long sum = (long)nums[0];
        for(int i = 1;i < nums.length;i++){
            preSum[i] = preSum[i-1] + (long)nums[i];
            
            sum += (long)nums[i];
        }
        long left = sum % p;
        if(left == 0) return 0;
        if(sum < p) return -1;
        
        int min = Integer.MAX_VALUE;
        
        for(int i = 0;i < preSum.length;i++){
            long l = i == 0?0: preSum[i-1];
            for(int j = i;j < preSum.length;j++){
                if((sum - preSum[j] + l) % p == 0 && sum != preSum[j]-l){
                    min = Math.min(min, j-i+1);
                }
            }
        }
        return min == Integer.MAX_VALUE?-1:min;
    }
}
```

### 复杂度

time: O(N)
space: O(N)

//bf
time: O(N^2)
space: O(N)