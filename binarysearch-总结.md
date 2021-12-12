### 模版

- 最右插入位置，最左大于(等于)target
- 367, 744
```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        
        while(left <= right){
            int mid = left + (right - left) / 2;
            
            if(nums[mid] < target){//如果只找插入位置，则可以合并=到这里，问题变为最左大于target
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else {//为了精确匹配到target
                return mid;
            }
        }
        
        return left;
    }
}
```

- 最左插入位置，最右小于target位置+1(如果等于target，自然也会匹配到)

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        
        while(left <= right){
            int mid = left + (right - left) / 2;
            
            if(nums[mid] >= target){
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        
        return left;
    }
}

- 精确匹配
852-能力，精确匹配

```