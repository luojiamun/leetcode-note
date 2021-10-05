### 思路

- 拿到题的直觉解法
    - 对每个数，每次乘以一个右边的邻居，一旦超过限制了，就停下来；

`AC`

- 答案最优解
    - 利用前缀和中间的一个思想，符合条件的子序列个数，其实等于序列的长度；
    - 那么可以利用这一点，扫一遍就可以得到结果了，
    - 一直向右扫，如果大于等于k了，就左边缩一下直到小于k
    - 每次因为都向右遇到了新尾巴，只要符合条件了，左右截取的这段长度就是符合条件的子序列的个数，直接加到结果里就好
    - 可以这么用，是因为a*b*c都符合条件的话，他的子序列都应该符合条件，因为这里的值都是正整数


### 代码
```java
//time O(N^2)
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int size = 0;
        for(int i = 0;i < nums.length;i++){
          int cur = i;
          int product = nums[cur];
          while(cur < nums.length && product < k){
            size++;
            cur++;
            if(cur < nums.length) product *= nums[cur];
          }
        }
        return size;
    }
}

//time O(N)
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int size = 0, product = 1, left = 0;
        for(int i = 0;i < nums.length;i++){
            product *= nums[i];
            while(product >= k && left <= i){
                product /= nums[left];
                left++;
            }
            size += i - left + 1;
        }
        return size;
    }
}
```


### 复杂度

//直接扫，暴力解
time: O(N^2)
space: O(1)

//双指针，前缀和思想
time: O(N)
space: O(N)