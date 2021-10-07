### 思路

- 拿到题的直觉解法
    - 借助堆，对窗口内的元素排序，只需注意每次开始时判断最大值是否还在范围内；

`AC`

- 看答案知道还有更优化的解法
    - 每次滑动的时候，就涉及到要往deque里面放新的元素；
    - 一旦放入，他就会参与下一轮的比大小
    - 为了保证可以被取出，如果会排出所有比新进来的元素小的，这样就保证了deque内是一个降序
    - 因为是降序，我们每次拿队首就肯定是最大的了
    - 有点类似单调栈的意思，单调q？单调队列

### 代码
```java
//heap
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int[] res = new int[nums.length - k + 1];
        if(nums.length == 1) return nums;
        PriorityQueue<int[]> heap = new PriorityQueue<>((o1, o2) -> o2[0]-o1[0]);
        
        for(int i = 0;i < k;i++){
            int[] cur = new int[]{nums[i], i};
            heap.add(cur);
        }
        int right = k-1;
        for(int i = 0;i < nums.length-k+1;i++){            
            while(heap.peek()[1] < i){
                heap.poll();
            }
            int[] max = heap.peek();
            res[i] = max[0];
            if(++right < nums.length){
                int[] rightItem = new int[]{nums[right], right};
                heap.add(rightItem);
            }
            
        }
        return res;
        
    }
}

//双指针
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 1) return nums;
        Deque<Integer> deq = new LinkedList<>();
        for(int i = 0;i < k;i++){
            prep(deq, nums, 0, i,k);
            deq.add(i);
        }
        
        int[] res = new int[nums.length - k + 1];
        res[0] = nums[deq.getFirst()];
        for(int left = 0, right = k;right < nums.length;right++,left++){
            prep(deq, nums, left, right,k);
            deq.add(right);
            res[left+1] = nums[deq.getFirst()]; 
        }
        return res;
    }
    
    public void prep(Deque<Integer> deq, int[] nums, int left, int right, int k){
        if(!deq.isEmpty() && deq.getFirst() == left && right >= k){
            deq.removeFirst();
        }
        
        while(!deq.isEmpty() && nums[right] > nums[deq.getLast()]){
            deq.removeLast();
        }
    }
}
```


### 复杂度

//heap
time: O(NlogN)
space: O(N)


//双指针
time: O(N)
space: O(N)