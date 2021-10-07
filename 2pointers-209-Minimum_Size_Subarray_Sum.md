### 思路

- 拿到题的直觉解法
    - 两个指针，一个前进，一个缩进；
    - 用了比较长的时间实现，没有官方答案简洁


`AC`


### 代码
```java
//官方答案
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0, min = Integer.MAX_VALUE, sum = 0;
        
        for(int i = 0;i < nums.length;i++){
            sum += nums[i];
            while(sum >= target){
                min = Math.min(min, i - left + 1);
                sum -= nums[left];
                left++;
            }
        }
        return min == Integer.MAX_VALUE?0:min;
    }
}

//原始
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        if(nums.length == 1){
            if(nums[0] == target){
                return 1;
            } else {
                return 0;
            }
        }
        int l = 0, r = 0, sum = nums[l], min = sum >= target?1:Integer.MAX_VALUE;
        while(r < nums.length){
            if(sum <= target){
                if(sum == target){
                    min = Math.min(min, r - l + 1);
                }
                r++;
                if(r < nums.length){
                    sum += nums[r];
                } 
            } else {
                if(l == r){
                    r++;
                    if(r+1 < nums.length){
                        sum += nums[r];
                    } 
                    min = Math.min(min, r - l + 1);
                }else{
                    min = Math.min(min, r - l + 1);
                    sum -= nums[l];
                    l++;
                }
            }
            
        }
        return min == Integer.MAX_VALUE?0:min;
    }
}
```


### 复杂度

//双指针
time: O(N)
space: O(1)