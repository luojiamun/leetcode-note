### 思路

- 一个堆

可以"暴力"得到结果。每次都弹出n/2(偶数的时候还有n/2 - 1)，求得median。使用完后还得从新push进去。
这么做可以完成部分测试用例，其他的会超时。

- 两个堆

主要利用`固定堆`的思想。

- 建立两个堆，一个大顶堆，一个小顶堆；

- 从结果上来看，小顶堆存储最大值，一个大顶堆存储最小值；

- 从大顶堆的底 -> 大顶堆的顶 -> 小顶堆的顶 -> 小顶堆的底，可以得到从小到大的完整序列；

- 保持小顶堆与大顶堆的size差距不超过1。

- 满足这些条件后，实际上被push进入两个堆的元素，实现了按大小平均存放在了两个堆中，且两个堆的顶部一定包含了中位数；

- 如果是奇数size，那么取小顶堆的顶；如果是偶数size，取两个顶的和/2;

- 关键是，如何实现这些要求；主要的技巧在push；
    - 无论何时，都push到min_heap;每次push之后，看两个条件决定是否进行挪移：
    - 条件1: 如果min_heap比max_heap多出两个了，将min_heap顶部挪到max_heap(挪过去的是最小的)；挪完之后两个堆size一样了；
    - 条件2: 如果min_heap只比max_heap多出一个，但min_heap顶比max_heap顶要小，那么换顶(先把min_heap顶挪到max_heap，再把max_heap顶挪到min_heap)，这样保证min_heap所有元素都比max_heap大(刚才的腾挪是把min_heap最小的和max_heap最大的做了交换)；(`这一步主要是因为新来的元素如果不腾挪，可能会不满足min_heap都是最大值这个要求`)


### 代码
```java
class MedianFinder {
    
    PriorityQueue<Integer> min_heap;
    PriorityQueue<Integer> max_heap;

    /** initialize your data structure here. */
    public MedianFinder() {
        this.min_heap = new PriorityQueue<>();
        this.max_heap = new PriorityQueue<>((a,b) -> b -a);
    }
    
    public void addNum(int num) {
        min_heap.add(num);
        
        if(min_heap.size() - max_heap.size() > 1){
            max_heap.add(min_heap.poll());
        }
        
        if(!max_heap.isEmpty() && min_heap.size() > max_heap.size() && min_heap.peek() < max_heap.peek()){
            max_heap.add(min_heap.poll());
            min_heap.add(max_heap.poll());
        }
        
    }
    
    public double findMedian() {
        if((min_heap.size() + max_heap.size()) % 2 == 0){
            return (min_heap.peek() + max_heap.peek()) * 1.0 / 2;
        } else {
            return min_heap.size() > max_heap.size()?min_heap.peek():max_heap.peek();
        }
    }
}
```

### 复杂度

addNum: O(logN);
findMedian: O(1)

space: O(N)
