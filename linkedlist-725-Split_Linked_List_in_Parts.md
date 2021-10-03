### 思路

- 拿到题的直觉解法
    - 主要问题不在链表，而在如何切分
    
`AC`

### 代码
```java
class Solution {
    public ListNode[] splitListToParts(ListNode head, int k) {
        //find the size of linked list
        ListNode cur = head;
        int count = 0;
        while(cur != null){
            cur = cur.next;
            count++;
        }
        
        //split based on the rule
        ListNode[] res = new ListNode[k];
        
        cur = head;
        for(int i = 0;i < res.length;i++){
            if(cur == null){
                res[i] = null;
                continue;
            } else {
                res[i] = cur;
            }
            if(count <= res.length){
                ListNode temp = cur.next;
                cur.next = null;
                cur = temp;
            } else {
                int groupCount = i < count%k?(count / k + 1):(count/k);
                while(groupCount > 1){
                    cur = cur.next;
                    groupCount--;
                }
                ListNode temp = cur.next;
                cur.next = null;
                cur = temp;
            }
            
        }
        return res;
    }
}
```

### 复杂度

//heap
time: O(N);
space：O(1)

//hashmap+ArrayList<Integer>[]
time: 没有用到排序，O(N)
space: O(N),比起上一种方法，多用了array of list，但并未影响最终复杂度；