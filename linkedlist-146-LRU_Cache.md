### 思路

- 拿到题的直觉解法

    - 第一遍审题错误。以为value就代表了频率，因此误认为默认要删除value高的值，用的heap去做。仔细看了下解释。
    - 题目的关键在于，优先删除的条件：一直没有操作的最早的那个元素；操作包括put和get，因此每个元素可以严格按时间排序。
    - get操作的目的很容易达到，维护一个hashmap，按照kv pair就可以，难点在于在默认capacity已满载的情况下，如何按照优先顺序删除，且时间要求O(1)；
    - hashmap可以保证查询get时间O(1)；需要一个数据结构使得优先列表的也可以在O(1)找到最靠前的元素；
    - 如果仅仅用List存储，没法随机找到key。理想状态是用额外的一个可以保证顺序的map来实现，<key, ...>，且这个结构可以将队内任意位置放到队尾且O(1);
    - 想到了linkedlist，但挪动的操作特别复杂；
    
    `failed to implement`

- 看答案思路，使用Doubled Linked List，自己实现

    - 同时拥有prev和next指针
    - 同时维护head和tail
    - 基于以上两点，随即挪动元素到末尾，只需要常数级别的操作了
    - 将<key, node>放入map，这样可以访问链表的node
    - 每次get，将对应node移到链表尾部
    - 每次put
        - 如果存在node，则更新map并挪动node到尾部
        - 如果不存在node
            - 如果超过capacity，删除链表头部元素，并删除map内对应key. **这里容易忘记**
            - 建<key, node>，put入map，将node放入链表尾部. **这里容易忘记**

    
    `AC`
    bug-free
    - 容易出问题的地方很多，边界用dummy来处理，但是上述思路需要先写清楚再写代码；

### 代码
```java

class LRUCache {
    int capacity;
    DLLNode dummyHead;
    DLLNode dummyTail;
    HashMap<Integer, DLLNode> map;
    
    private class DLLNode{
        int key;
        int value;
        
        DLLNode next;
        DLLNode prev;
        
        public DLLNode(int key, int value){
            this.key = key;
            this.value = value;
        }
        
    }
   
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.dummyHead = new DLLNode(-100, -100);
        this.dummyTail = new DLLNode(-100, -100);
        dummyHead.next = dummyTail;
        dummyTail.prev = dummyHead;
        this.map = new HashMap<>();
    }
    
    public int get(int key) {
        if(!map.containsKey(key)){
            return -1;
        } else {
            DLLNode node = map.get(key);
            node.prev.next = node.next;
            node.next.prev = node.prev;
            moveToTail(dummyTail, node);
            return node.value;
        }
        
    }
    
    public void moveToTail(DLLNode tail, DLLNode node){
        node.prev = tail.prev;
        node.next = tail;
        tail.prev = node;
        node.prev.next = node;
    }
    
    public void put(int key, int value) {
        if(map.containsKey(key)){
            DLLNode node = map.get(key);
            node.value = value;
            node.prev.next = node.next;
            node.next.prev = node.prev;
            moveToTail(dummyTail, node);
            map.put(key, node);
        } else {
            if(map.size() >= capacity){
                DLLNode head = dummyHead.next;
                dummyHead.next = head.next;
                head.next.prev = dummyHead;
                map.remove(head.key);
                
            }
            DLLNode node = new DLLNode(key, value);
            moveToTail(dummyTail, node);
            map.put(key, node);
        }
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

### 复杂

get和put的时间复杂度: O(1)
整个解法的空间复杂度: O(N)，维护一个HashMap。

