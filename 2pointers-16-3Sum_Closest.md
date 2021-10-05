### 思路

- 拿到题的直觉解法
    - 暴力解法：三层循环，分别代表三个数，找到与target最小的组合.O(N^3)
- 官方优化解法
    - 先排序，O(NlogN)
    - 三指针：一个指针固定第一个元素，第二/三个指针做改进版的twoSum，每次都记录距离
    - 最后的res即为最近距离

`AC`


### 代码
```java
//time O(NlogN)
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int optimal = 1000000;
        for(int i = 0;i < nums.length;i++){
          int firstVal = nums[i];
          int left = i + 1, right = nums.length - 1;

          while(left < right){
            int secVal = nums[left];
            int thirdVal = nums[right];
            int sum = firstVal + secVal + thirdVal;
            if(Math.abs(sum - target) < Math.abs(optimal - target)){
              optimal = sum;
            }
            if(sum < target){
              left++;
            } else if(sum == target){
              return sum;
            } else {
              right--;
            }
          }
        }
        return optimal;
    }
}
```


### 复杂度

time: O(NlogN)
space: O(1)
