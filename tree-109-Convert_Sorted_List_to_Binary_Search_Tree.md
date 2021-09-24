### 思路

- 拿到题的直觉解法

链表的中点是树的root，左半是左子树的全部节点，右半是右子树的全部节点；

链表的中点可以通过快慢指针找到；

可以用递归(pre-order dfs)来生成树，递归的结构
- params: head of listnode
- return：root of tree
- 递归终止条件：head of listnode is null

注意：影响bug free的问题
- 注意左子树。使用了dummy后，设置leftHead的时候要使用dummy.next而不是直接使用head，因为要考虑到当前head只有一个node，它的左右分枝因此都为null，如果直接使用head，会导致多出来了分枝。dummy.next可以很`智能`的规避这个问题。
- 可以用dummy做pre，返回dummy.next很方便；

`AC`

- 官网有意思的解法

BST的in-order顺序和linkedlist顺序是一致的，既然如此，我们就可以匹配这两者。但题目是生成树，因此在没有树的时候，我们如何来in-order dfs？
这个解法巧妙的模拟了in-order，即便没有树，也可以dfs，只要保持相对同步就行。使用的工具是左右子树在linkedlist的index。

`AC`

### 代码
```java
//pre-order dfs recursion
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null){
            return null;
        }
        if(head.next == null){
            TreeNode root = new TreeNode(head.val);
            return root;
        }
        
        return helper(head);
        
    }
    
    public TreeNode helper(ListNode head){
        if(head == null) return null;
        //find mid node in the listnodes
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode fast = dummy;
        ListNode slow = dummy;
        while(fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        //slow.next is root, then slow should point to null
        ListNode rootNode = slow.next;
        TreeNode root = new TreeNode(rootNode.val);
        slow.next = null;
        ListNode leftHead = dummy.next;
        ListNode rightHead = rootNode.next;
        root.left = helper(leftHead);
        root.right = helper(rightHead);
        
        return root;
    }
}

//边模拟in-order, 边生成
class Solution {
    ListNode head;
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null) return null;
        if(head.next == null) {
            TreeNode root = new TreeNode(head.val);
            return root;
        }
        
        this.head = head;
        int length = 0;
        ListNode cur = head;
        while(cur != null){
            length++;
            cur = cur.next;
        }
        
        return inorder(0, length);
    }
    
    public TreeNode inorder(int left, int right){
        if(left > right || head == null) return null;
        int mid = left + (right - left) / 2;
        TreeNode l = inorder(left, mid - 1);
        
        TreeNode root = new TreeNode(head.val);
        head = head.next;
        
        root.left = l;
        root.right = inorder(mid + 1, right);
        
        return root;
    }
}
```

### 复杂度

`pre-order dfs`
time: O(NlogN), 递归的部分会有logN次，每次会有快慢指针N/2次，因此总量N * longN
space: O(logN),主要是递归用来存储栈的占用。

`simulate in-order`
time: O(N)，递归的复杂度只有logN，但我们要通过一次遍历链表查询长度，因此复杂度最高为N
space: O(logN)，主要是递归存储栈的占用。