### 思路

- 拿到题目的原始思路（没有做出来）

用一个pre记录当前起伏长度；用一个flip记录前一个是`起`还是`伏`;
如果当前值与下一位的起伏状态与flip不同，那么意味着pre可以+1；
否则pre复位到2；

注意一个特殊情况就是如果出现相等的元素，那pre=1；

`AC`

- 滑动窗口

左侧指针指向起点，当前值为终点；

如果当前值的左右两侧造成起伏，那意味着当前值符合条件，continue；

如果无法造成起伏，那么起伏状态结束了，记录长度；

和上种方法一样，注意一个特殊情况，如果出现相等的元素，那应该忽略掉，把左侧指针指向当前值；
(因为默认的长度至少就为1.但凡arr中不是所有元素都一样，长度就会为2.所以相等时的判读就不需要操作ans了。)
 
`AC`

### 代码
```java

//array
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        if(arr.length == 1) return 1;
        
        int pre = 0;
        int ans = Integer.MIN_VALUE;
        boolean flip = arr[0] > arr[1]?true:false;
        
        for(int i = 0;i < arr.length - 1;i++){
            
            if(arr[i] == arr[i+1]){
                pre = 1;
                if(i + 2 < arr.length) flip = arr[i+1] > arr[i+2];
            } else {
                if(flip != arr[i] > arr[i+1]){
                    pre++;
                } else {
                    pre = 2;
                }
                flip = arr[i] > arr[i+1];
            }
            ans = Math.max(ans, pre);
        }
        
        return ans;
    }
}

//滑动窗口
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        int N = arr.length;
        int left = 0;
        int ans = 1;
        for(int i = 1;i < N;i++){
            if(Integer.compare(arr[i], arr[i - 1]) == 0){
                left = i;
                continue;
            }
            if(i == arr.length - 1 || Integer.compare(arr[i],arr[i+1]) * Integer.compare(arr[i - 1],arr[i]) >= 0){
                ans = Math.max(ans, i - left + 1);
                left = i;
            }
        }
        
        return ans;  
    }
}
```

### 复杂度

array
time: O(n)
space: O(1)

单调栈
time: O(N)
space: O(1)