### 思路

- 拿到题目的原始思路（没有做出来）

借助一个sorted，和原本的arr做比较。

如果arr当前的元素与sorted相同，ans++；

如果不同，则根据index追踪arr当前元素在sorted位置，经过几次迭代后直到返回当前元素的位置。此时可以得到一个最大(最靠右)的index，即为当前元素可以形成partition的边界。

然后在实现上，卡在了重复元素的处理上，花了很长时间。这个思路其实是可以，但是实现起来比较麻烦。

- 思路1：

借助一个排好序的sorted，和原本的arr做比较。

如果当前index之前的元素形成的集合是相同的，那么就可以算作一个partition。

相同在这里的定义是差的和为0。

之所以可以借用sum=0这个，是因为比较的对象是sorted，而不是如下这种反例。

arr1: [1,6,6,4,8]
sorted: [1,4,6,6,8]
反例：[1,4,8,6,6]

 - 思路2: 
 单调栈

这个思路，帮助进一步理解单调栈的使用。

- 如果一个序列可以完全partition，那么应该是sorted的；所以"正常情况"应该是递增的；
- 我们因此track给出的序列是否出现第一个递减的情况，如果出现了，就说明这个当前递减的和栈顶必须是一个partition的；
- 这样还不足够，继续得拿当前这个与栈内的其他元素做比较，如果比其他元素小，说明他们也比必须是一个partition的(因为小的在右边)
- 只要是当前栈内的元素大过cur的，都可以pop掉了，因为之前最开始pop出去的元素(最大)已经可以代表这个partition(original top, cur, cur_top)了
- 过程结束后，将original top压回来，因为它要作为代表，继续和后面的做比较，如果它大，说明后面这个也是属于该partition


### 代码
```java

//辅助sorted array
class Solution {
    public int maxChunksToSorted(int[] arr) {
        int[] sorted = Arrays.copyOf(arr, arr.length);
        Arrays.sort(sorted);
        int sum = 0;
        int ans = 0;
        for(int i = 0;i < sorted.length;i++){
            sum += arr[i] - sorted[i];
            if(sum == 0) ans++;
        }
        return ans;
    }
}

//单调栈
class Solution {
    public int maxChunksToSorted(int[] arr) {
        Stack<Integer> stack = new Stack<>();
        
        for(int i = 0;i < arr.length;i++){
            //注意这里的条件，要自己测试一下；需要严格单调才行；
            if(!stack.isEmpty() && stack.peek() > arr[i]){
                int max = stack.pop();
                while(!stack.isEmpty() && stack.peek() > arr[i]){
                    stack.pop();
                }
                stack.push(max);
            } else {
                stack.push(arr[i]);
            }
        }
        
        return stack.size();
    }
}
```

### 复杂度

辅助sorted array
time: O(nlogn);复杂度的主要来源是排序；
space: O(N),需要维护一个额外的sorted array；

单调栈
time: O(N)
space: O(N)，需要维护一个额外的栈；