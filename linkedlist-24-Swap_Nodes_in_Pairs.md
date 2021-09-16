### 思路

- 拿到题目的思路

1. 每两个node为一个pair进行交换；
2. 交换完成后，next到下一对pair；

bug free需要注意的地方：

1. 交换完成后，和这一对pair的pre的关系要接上；
2. 可以用dummy做pre，返回dummy.next很方便；

`AC`

### 代码
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head == null || head.next == null) return head;
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;
        ListNode left = head;
        ListNode right = head.next;
        
        
        while(left != null && right != null){
            left.next = right.next;
            right.next = left;
            pre.next = right;
            pre = left;
            left = left.next;
            if(left != null){
                right = left.next;
            }    
        }
        return dummy.next;
    }
}
```

### 复杂度

time: O(N)
space: O(1)