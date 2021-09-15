### 思路

- 拿到题目的思路

1. 遍历一次找到链表长度，记住old_tail；
2. 遍历第二次停在length-k，此处为new_tail，next为新的head
3. 把new_tail指向null，old_tail指向head，返回new_head;

注意影响bug free的因素：
边界问题。k可长可短，如果为0怎么办；如果超过length怎么办，如果超过length但是回头了(%length == 0)怎么办?
1. 如果k=0，可以提前return；
2. 如果`k % length = 0`,这个`一定要return`,否则会有错误答案。

`AC`

### 代码
```java

class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if(head == null || head.next == null || k == 0) return head;
        int length = 0;
        ListNode cur = head;
        while(cur.next != null){
            cur = cur.next;
            length++;
        }
        length++;
        ListNode old_tail = cur;
        
        if(k % length == 0) return head;
        
        cur = head;
        int count = length - k % length;
        
        while(--count > 0){
            cur = cur.next;
        }
        
        ListNode new_head = cur.next;
        cur.next = null;
        old_tail.next = head;
        
        return new_head;
    }
}
```

### 复杂度

time: O(N)
space: O(1)