### 思路

- 拿到题目的原始思路（没有做出来）

借助一个sorted，和原本的arr做比较。

如果arr当前的元素与sorted相同，ans++；

如果不同，则根据index追踪arr当前元素在sorted位置，经过几次迭代后直到返回当前元素的位置。此时可以得到一个最大(最靠右)的index，即为当前元素可以形成partition的边界。

然后在实现上，卡在了重复元素的处理上，花了很长时间。这个思路其实是可以，但是实现起来比较麻烦。

- 答案思路：

借助一个排好序的sorted，和原本的arr做比较。

如果当前index之前的元素形成的集合是相同的，那么就可以算作一个partition。

相同在这里的定义是差的和为0。

之所以可以借用sum=0这个，是因为比较的对象是sorted，而不是如下这种反例。

arr1: [1,6,6,4,8]
sorted: [1,4,6,6,8]
反例：[1,4,8,6,6]



### 代码
```java
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
```

### 复杂度

push: O(nlogn);复杂度的主要来源是排序；
space: O(N),需要维护一个额外的sorted array；
