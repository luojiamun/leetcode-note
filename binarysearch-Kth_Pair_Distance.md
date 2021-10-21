### 思路

- 拿到题的直觉解法
    - 这题首先想到的不是二分，是heap。因为Kth min这个关键词，联想到了堆，可以建一个固定k大小的大顶堆，放入所有可能的abs(x-y)可以得到结果。但这样做的复杂度太高。首先要遍历每种可能，O(N^2)，每种可能下要入堆O(logN),总共O(N^2logN)的时间复杂度，O(K)的空间复杂度。`TLE`
    - 其次，想了很久怎么是尝试二分，能想到首先肯定是要排序的，O(NlogN)；
    - 然后能想到是会用到双指针的，用来找第k小。但是也就卡在这里，想不出来了。

`Failed`

- 排序这一点是需要的。
- 难点在于，看出来这是一个典型的`计数二分`.
    - 思路转换：求kth最小 => `求一个条件(一个数),使得原arr中有k个符合条件的元素`
    - 这种思路转换需要练习，就像是前缀和把求和问题变成了求差问题，变成atMost问题(通过<=k - <=k-1来找k)。
- 思路转换过来后，这题变成了`给你一个sorted arr，找到一个数p，使得arr中元素的abs(x-y)<=p的组合有k个`
- 还是不够直白。`当p足够大arr,比如 >= arr[n-1] - arr[0]，那所有的abs(x-y)组合都会落入袋中，而随着p降低，落入条件范围的元素会减少。`
这实际上是在一个`[符合条件的{abs(x-y)的个数0, p0}, {符合条件的{abs(x-y)的个数1, p1}...]`的序列中做二分，其中`符合条件的{abs(x-y)的个数i`是一个non-decreasing序列。那么这到底是个什么模版？
- 假如说这个`符合条件的{abs(x-y)的个数i`是[1,2,2,4,4,5]我们的k=3。注意这里的问题是一个确定性的能力二分问题。能力二分的函数是`找到<=p的组合个数`，二分target为k。我们的k不一定在个数序列中。比如这个例子里的target=3。所以这里的模版是一个典型的最左插入位置问题。也就是下面代码中的样子。当然为了锻炼思路，你说它是最右插入位置也是可以，但是模版也要改，当做第三个解法放在下面的代码里。但这种解法要改模版，因此不可取。

### 代码
```java
//heap，超时
import java.util.*;

class Solution {
    public int solve(int[] nums, int k) {
        PriorityQueue<Integer> res = new PriorityQueue<>((o1, o2) -> o2-o1);

        for(int i = 0;i < nums.length;i++){
            for(int j = i+1;j < nums.length;j++){
                res.add(Math.abs(nums[i] - nums[j]));
                if(res.size() > k+1) res.poll();
            }
        }

        return res.poll();
    }
}
//最左插入位置
import java.util.*;

class Solution {
    public int solve(int[] nums, int k) {
        Arrays.sort(nums);
        k++;
        int left = 0, right = nums[nums.length - 1] - nums[0];

        while(left <= right){
            int mid = left + (right - left) / 2;

            if(getSmallerThanMid(nums, mid) >= k){
                right = mid - 1;
            } else {
                left = mid + 1;
                
            }
        }
        
        
        return left;

    }

    public long getSmallerThanMid(int[] nums, int mid){
        long res = 0;
        int left = 0;
        for(int i = 1;i < nums.length;i++){
            while(nums[i] - nums[left] > mid){
                left++;
            }
            res += i - left;
        }
        return res;
    }
}

//最右插入位置
import java.util.*;

class Solution {
    public int solve(int[] nums, int k) {
        Arrays.sort(nums);
        // k++;
        int left = 0, right = nums[nums.length - 1] - nums[0];

        while(left <= right){
            int mid = left + (right - left) / 2;

            if(getSmallerThanMid(nums, mid) > k){
                right = mid - 1;
            } else {
                left = mid + 1;
                
            }
        }
        
        
        return right+1;//这里注意

    }

    public long getSmallerThanMid(int[] nums, int mid){
        long res = 0;
        int left = 0;
        for(int i = 1;i < nums.length;i++){
            while(nums[i] - nums[left] > mid){
                left++;
            }
            res += i - left;
        }
        return res;
    }
}

```


### 复杂度

time: 
- 排序：O(NlogN), where N = nums.length
- 二分：O(logN)次，每次需要O(N)来能力检测，总的O(NlogN)
总的O(NlogN)

space: O(1)