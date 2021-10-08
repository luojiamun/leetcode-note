### 思路

- 拿到题的直觉解法
    - 直接缩进的写法是不行的，因为没法考虑到[2,2,1,2,2]这种两端都可以伸缩的情况；
    - 求最大长度的题，很对应伸缩方法，但求个数的题则不是；
    - 这题可以用前缀和，但对前缀和的理解也要深入
    - 前缀和，总的来说是把符合条件的情况从左到右累积一下
    - 但首先得知道当前位置符合条件的为多少
    - 简单的前缀和，默认都是可以在前一个基础上累加的，但这题不是，不能直接pre++
    - 每个位置的和由其符合条件的长度决定


### 代码
```java
//直觉解法
class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        return atMost(nums, k) - atMost(nums, k-1);
    }
    
    public int atMost(int[] nums, int k){
        int[] presum = new int[nums.length + 1];
        int left = 0, sum = 0;
        for(int i = 1;i < presum.length;i++){
            int cur = nums[i-1];
            if(cur % 2 != 0) sum++;
            while(sum > k){
                if(nums[left] % 2 != 0) sum--;
                left++;
            }
            presum[i] = i - left + 1 + presum[i - 1];
        }
        return presum[nums.length];
    }
}
```


### 复杂度

time: O(N)
space: O(N)