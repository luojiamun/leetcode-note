### 思路

- 拿到题的直觉解法
    - 快慢指针经典题
    - 不需要找到cycle起点，只是看有没有cycle

`AC`


### 代码
```java
//time O(N) space O(1)
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null) return false;
    
        ListNode dummy = new ListNode(-1);

        dummy.next = head;

        ListNode slow = dummy, fast = dummy;

        do {
          slow = slow.next;
          fast = fast.next;
          if(fast != null) fast = fast.next;
        }while(slow != null && fast != null && fast != slow);
        

        if(fast == null || slow == null) {
          return false;
        } else {
          return true;
        }
    }
}
```

### 复杂度

//双指针
time: O(N)
space: O(1)