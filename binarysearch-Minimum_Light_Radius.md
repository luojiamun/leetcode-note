### 思路

- 拿到题的直觉解法
    - 这题花了不少时间才读懂题目意思，主要卡在了，错误地觉得每个灯要更多的照到房子才能最优，于是走进了总长/3的错误路线。
    - 其实没必要想那么多，题目的意思翻译过来就是：从左到右摆灯，摆三个，必须照到所有房子。
    - 那这样的情况下，其实二分是很容易想到的：二分在这里的意思是，不断尝试半径，直到符合条件。
    - 第二个卡住的地方是，灯怎么摆？又想多了。
    - 其实每摆一个灯，只要下一个房子照不到，那就再摆个灯在房子右边就是最好的了，因为前一个房子已经照到了。
    - 理解了题目意思，二分的部分其实就是套模板。
    - 这里是`能力二分`，`寻找最左符合条件的数`。因为我们要找最小，所以是最左；照亮，就是符合条件。

`AC`


### 代码
```java
import java.util.*;

class Solution {
    public double solve(int[] nums) {
        if(nums.length <= 3) return 0;
        Arrays.sort(nums);

        int l = 0, r = nums[nums.length - 1];

        while(l <= r){
            int m = l + (r - l) / 2;
            
            if(find(m, nums)){
                r = m - 1;
            } else {
                l = m + 1;
            }
        }

        if(l > nums[nums.length - 1] || !find(l, nums)){
            return -1;
        }
        return l / 2.0;
    }

    public boolean find(int mid, int[] nums){
        int range = nums[0]+mid, lights = 0;
        
        for(int i = 0;i < nums.length;i++){
            if(nums[i] > range){
                lights++;
                range = nums[i] + mid;
            }
        }
        

        return lights <= 2;
    }
}
```


### 复杂度

time: 
- 排序：O(NlogN), where N = nums.length
- 二分：O(log(Max - Min)), where Max = Math.max(nums), Min = Math.min(nums);每次二分都需要find,最坏O(N);总的O(Nlog(Max-Min))
总的O(NlogN + Nlog(Max-Min)

space: O(1)