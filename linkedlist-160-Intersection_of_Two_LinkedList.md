### 思路

- 拿到题的直觉解法

两个LinkedList如果长度一样，那么直接遍历就好；

两个LinkedList如果长度不同，则先算长短，然后长的先走length的差，就可以齐头并进当作相同长度的case处理；

`AC`

- 稍微优化

两个LinkedList不用分别求长度，要知道的只是
- 长度差
更进一步，连长度差都不需要知道，只需要长的先走diff那几步，所以可以稍微优化一下这部分。复杂度没变。

`AC`

### 代码
```java

//遍历两次求长度
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lengthA = 0;
        ListNode curA = headA;
        while(curA != null){
            curA = curA.next;
            lengthA++;
        }
        int lengthB = 0;
        ListNode curB = headB;
        while(curB != null){
            curB = curB.next;
            lengthB++;
        }
        
        ListNode headLong = lengthA >= lengthB?headA: headB;
        ListNode headShort = lengthA >= lengthB?headB: headA;
        int diff = Math.abs(lengthA - lengthB);
        
        while(diff > 0){
            headLong = headLong.next;
            diff--;
        }
        
        while(headLong != headShort){
            headLong = headLong.next;
            headShort = headShort.next;
        }
        
        return headLong;
    }
}

//遍历一次
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode curA = headA;
        ListNode curB = headB;
        
        while(curA != null && curB != null){
            curA = curA.next;
            curB = curB.next;
        }
        
        ListNode shortee = curA == null?headA: headB;
        ListNode longee = curA == null?headB: headA;
        ListNode running = curA == null?curB: curA;
        
        int diff = 0;
        while(running != null){
            running = running.next;
            longee = longee.next;
        }
        while(shortee != null && longee != null && shortee != longee){
            shortee = shortee.next;
            longee = longee.next;
        }
        return shortee;
    }
}
```

### 复杂

两个解法复杂度一样，但是第二个方法因为少遍历了一些所以稍微优化点；
time: O(N)
space: O(1)

