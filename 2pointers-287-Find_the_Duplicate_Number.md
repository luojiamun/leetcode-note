### 思路

- 拿到题的直觉解法
    - 在没有额外空间且不许动nums的基础上，需要找到，可以追踪值和index之间的关系，如果出现循环就有问题；但这个不知道怎么存储和利用；
    - 如果对空间无限制，可以用hashmap
    - 如果对时间无限制，可以sorted，不就可以轻松找了么；

`AC`

- 答案最优解
    - 利用链表中的环的思路


### 代码
```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0], fast = nums[0];
        
        do{
            slow = nums[slow];
            fast = nums[nums[fast]];
        }while(slow != fast);
        
        fast = nums[0];
        while(slow != fast){
            slow = nums[slow];
            fast = nums[fast];
        }
        
        return slow;
    }
}
```


### 复杂度

//双指针，链表环
time: O(N)
space: O(1)