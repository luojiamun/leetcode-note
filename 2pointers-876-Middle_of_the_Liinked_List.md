### 思路

- 拿到题的直觉解法
    - 快慢双指针
    - 1/2，搭配，刚好切分原长度的一半
    - 注意偶数个时题目选择了右侧的，这个需要通过设定快指针的位置来看；
    - 如果题目选择左侧，那么要设定while的结束条件为fast.next != null，提前一步结束；

`AC`


### 代码
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        if(head.next == null) return head;
    
        ListNode dummy = new ListNode(-1);

        dummy.next = head;

        ListNode slow = dummy, fast = dummy;

        while(fast != null){
          slow = slow.next;

          fast = fast.next;
          if(fast != null){
            fast = fast.next;
          }
        }

        return slow;
    }
}
```

### 复杂度

time: O(N)
space: O(1)