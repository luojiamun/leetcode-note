### 思路

- 拿到题的直觉解法
    - 二分的题，目标是将题目匹配到模板；
    - 没匹配，就没想清楚，做出来也没意义；下次还是错；
    - 题目的意思是，找到第一个bad，并非找到bad即可，所以如果将问题定义为`找到bad`，则不是一个精确匹配的题，而是一个找最左匹配的题；
    - 根据题目条件，可以匹配的模版是：
    `寻找最左符合条件的值；第一个bad`
    - 当然，如果非要用精确匹配的最简单模版，也是可以的，那要把问题定义为`找到第一个bad`，这就是精确匹配了，但是`第一`得需要进一步在判断条件中定义清楚
    `找到一个指定的bad，它的左侧要么为空，要么为good`

最好使用最做匹配，因为精确匹配需要注意的edge case更多。
`AC`


### 代码
```java
//寻找最左符合条件的值
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int left = 0, right = n;
        
        while(left <= right){
            int mid = left + (right - left) / 2;
            
            if(!isBadVersion(mid)){
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        if(left > n || !isBadVersion(left)){
            return -1;
        }
        return left;
    }
}

//精确匹配
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int left = 1, right = n;
        
        while(left <= right){
            int mid = left + (right - left) / 2;
            
            if(isBadVersion(mid) && (mid - 1 == 0 || !isBadVersion(mid - 1))){
                return mid;
            } else if(isBadVersion(mid)) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        
        return -1;
    }
}
```


### 复杂度

time: O(logN)
space: O(1)