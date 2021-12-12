### 思路

- 拿到题的直觉解法
    - 固定k小顶堆模版题

`AC`

### 代码
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> heap = new PriorityQueue<>();
        
        for(int i = 0;i < nums.length;i++){
            if(heap.size() < k){
                heap.add(nums[i]);
            } else if(heap.peek() < nums[i]){
                heap.poll();
                heap.add(nums[i]);
            }
        }
        return heap.poll();
    }
}
```

### 复杂度

time: O(N*logN)
space: O(K)