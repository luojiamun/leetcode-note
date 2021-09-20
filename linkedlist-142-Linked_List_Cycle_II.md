### 思路

- 拿到题的直觉解法

经典的双指针跑圈问题；

    - 建dummy node
    - fast,slow双指针同时从dummy出发
    - 如果到头了，return null
    - 如果相遇了，slow指针不动，fast指针回dummy
    - fast，slow再次同时从当前位置出发，且都以慢速前进
    - 再次相遇地即为环的起点

    影响bug-free的地方
    - dummy要有，减少边界判断
    - 第一次跑圈的条件，slow == fast不能放在while(如果都从dummy出发)，也不能放在开头，要放在结尾，避免逻辑错误；
    - dummy虽然能覆盖边界，但还是要自我检测，不然有时候有些边界会漏掉

`AC`

### 代码
```java

public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode fast = dummy;
        ListNode slow = dummy;
        
        while(slow != null && fast!= null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
            if(slow == fast) break;
        }
        
        if(slow == null || fast == null || fast.next == null) return null;
        
        fast = dummy;
        
        while(slow != fast){
            slow = slow.next;
            fast = fast.next;
        }
        
        return slow;
    }
}
```

### 复杂

time: O(N)
space: O(1)

