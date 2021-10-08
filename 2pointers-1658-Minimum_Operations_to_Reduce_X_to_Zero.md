### 思路

- 拿到题的直觉解法
    - 因为左右两边都可能缩进，没想到什么办法可以线性推进，看起来像是backtrack的问题
    - 使用递归backtrack可以做，相当于暴力解，TLE

`TLE`

- 答案解法
    - 依旧使用双指针，依旧从左到右，依旧缩进
    - 为什么没看出来？因为，我们要盯着的不是窗口内的元素，而是窗口外的元素和；
    - 双指针问题，也有可能是这种，看着窗口外的元素的情况；
    - 找窗口内的元素和sum问题 === 全体元素和sum - 窗口外元素和sum，`可以切换问题`


### 代码
```java
//递归
class Solution {
    public int minOperations(int[] nums, int x) {
        Deque<Integer> deq = new LinkedList<>();
        long sum = 0;
        for(int i: nums) {
            deq.addLast(i);
            sum += i;
        }
        if(sum < x) return -1;
        int res = helper(deq, 0, x, nums.length);
        return res == Integer.MAX_VALUE?-1:res;
    }
    
    public int helper(Deque<Integer> deq, int sum, int target, int length){
        if(sum == target) return length - deq.size();
        if(deq.size() == 0) return Integer.MAX_VALUE;
        if(sum > target) return Integer.MAX_VALUE;
        
        // System.out.println(sum);
        int first = deq.pollFirst();
        int l = helper(deq, sum+first, target, length);
        deq.addFirst(first);
        int last = deq.pollLast();
        int r = helper(deq, sum+last, target, length);
        deq.addLast(last);
        
        return l<r?l:r;
    }
}

//双指针非固定滑动窗口
class Solution {
    public int minOperations(int[] nums, int x) {
        long sum = 0, res = Integer.MAX_VALUE;
        for(int i: nums){
            sum += i;
        }   
        if(sum < x) return -1;
        if(sum == x) return nums.length;
        int left = 0, window = 0;
        for(int i = 0;i < nums.length;i++){
            window += nums[i];
            while(window > sum - x){
                window -= nums[left++];
            }
            if(window == sum - x) res = Math.min(res, nums.length - i - 1 + left);
        }
        return (int)res == Integer.MAX_VALUE?-1:(int)res;
        
    }
}
```


### 复杂度

//backtrack
O(2^N)
O(N)

//presum
O(N)
O(1)