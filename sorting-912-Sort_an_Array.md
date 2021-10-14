### 思路

- 拿到题的直觉解法
    - 解法很多
    - 第一个选择MergeSort，有不错时间复杂度
    - 先拆分，再合并，Divide and Conquer
    - 第二个选择分桶，因为题目给的数字有限，直接给建一个array装起来再排序

`AC`


### 代码
```java
//merge sort
class Solution {
    public int[] sortArray(int[] nums) {
        mergeSort(nums, 0, nums.length - 1);
        return nums;
    }
    
    public void mergeSort(int[] nums, int start, int end){
        if(end - start == 0) return;
        if(end - start == 1) {
            if(nums[end] >= nums[start]) {
                return;
            } else {
                int temp = nums[end];
                nums[end] = nums[start];
                nums[start] = temp;
                return;
            }
        }
        int mid = start + (end - start) / 2;
        mergeSort(nums, start, mid);
        mergeSort(nums, mid+1, end);
        merge(nums, start, mid, end);
    }
    
    public void merge(int[] nums, int start, int mid, int end){
        int[] temp = new int[end - start + 1];
        int left = mid, right = end;
        for(int i = temp.length - 1;i >= 0;i--){
            if(left < start || (right > mid && nums[right] >= nums[left])){
                temp[i] = nums[right--];
            } else {
                temp[i] = nums[left--];
            }
        }
        System.arraycopy(temp, 0, nums, start, temp.length);
    }
}

//bucket
class Solution {
    public int[] sortArray(int[] nums) {
        int[] temp = new int[50000*2+1];
        for(int i = 0;i < nums.length;i++){
            temp[nums[i]+50000] += 1;
        }
        int p = 0;
        for(int i = 0;i < temp.length;i++){
            while(temp[i] > 0){
                nums[p++] = i-50000;
                temp[i]--;
            }
        }
        return nums;
        
    }
}
```


### 复杂度

//merge sort
time: O(NlogN),因为二分了，因此有logN次，每次，合并两个片段最多n个数，O(N)，因此加起来就是O(NlogN)
space: O(N),再合并时，需要一个辅助的数组帮助合并，而不是在数组本身操作。

//bucket
time: O(N)
space: O(N)