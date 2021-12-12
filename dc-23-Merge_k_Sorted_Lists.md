### 思路

- 拿到题的直觉解法
    - merge sort的链表版；注意一定要采取logN的合并方式才是归并，不要暴力合并；
    - 或者暴力的用堆
    - 或者优化的用堆(没写)，类似于多路归并，把每个head都加进去，poll最小的，然后next再放回去，不断迭代直到全部放入；因为堆里的元素是固定的所以时间复杂度更优；

`AC`


### 代码
```java
//merge
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length == 0) return null;
        ListNode head = lists[0];
        for(int i = 1;i <= Math.log(lists.length)/Math.log(2) + 1.;i++){
            for(int j = 0;j < lists.length;j += Math.pow(2, i)){
                if(j+(int)Math.pow(2, i-1) < lists.length)
                    lists[j] = merge(lists[j], lists[j+(int)Math.pow(2, i-1)]);
            }
        }
        return lists[0];
    }
    
    public ListNode merge(ListNode head1, ListNode head2) {
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy; 
        // System.out.println("start");
        while(head1 != null || head2 != null){
            if(head1 == null) {
                cur.next = head2;
                cur = cur.next;
                head2 = head2.next;
                continue;
            }
            if(head2 == null) {
                cur.next = head1;
                cur = cur.next;
                head1 = head1.next;
                continue;
            }
            if(head1.val < head2.val){
                cur.next = head1;
                head1 = head1.next;
                cur = cur.next;
            } else {
                cur.next = head2;
                head2 = head2.next;
                cur = cur.next;
            }
            // System.out.println(cur.val);
        }
        return dummy.next;
    }
}

//heap,BF
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        PriorityQueue<Integer> p = new PriorityQueue<>();
        
        for(ListNode ln: lists){
            for(ListNode node = ln;node != null;node = node.next){
                p.offer(node.val);
            }
        }
        
        if(p.isEmpty()){
            return null;
        }
        
        ListNode head = new ListNode(p.poll());
        ListNode cur = head;
        while(!p.isEmpty()){
            cur.next = new ListNode(p.poll());
            cur = cur.next;
        }
        return head;
    }
}
```


### 复杂度

//merge
time: O(KNlogK)
space: O(1),没用递归
//heap
N is total number of nodes in all LinkedList
time: O(NlogN)
space: O(N)