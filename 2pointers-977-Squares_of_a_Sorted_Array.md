### 思路

- 拿到题的直觉解法
    - 直接计算平方，然后sort
- 优化时间解法
    - 建一个新array
    - 双指针两头扫

`AC`


### 代码
```java
//time O(NlogN) space O(1)
class Solution {
    public int[] sortedSquares(int[] nums) {
        for(int i = 0;i < nums.length;i++){
          nums[i] = nums[i] * nums[i];
        }
        Arrays.sort(nums);

        return nums;
    }
}

//time O(N) space O(N)
class Solution {
    public int[] sortedSquares(int[] nums) {
        if(nums[0] >= 0){
          for(int i = 0;i < nums.length;i++){
            nums[i] = nums[i] * nums[i];
          }
          return nums;
        }

        int[] res = new int[nums.length];

        int l = 0, r = nums.length - 1, cur = nums.length - 1;

        while(l <= r){
          if(nums[l] * nums[l] < nums[r] * nums[r]){
            res[cur--] = nums[r] * nums[r];
            r--;
          } else {
            res[cur--] = nums[l] * nums[l];
            l++;
          }
        }

        return res;
    }
}
```


### 复杂度

//sort
time: O(NlogN)
space: O(1)

//双指针，空间换时间
time: O(N)
space: O(N)