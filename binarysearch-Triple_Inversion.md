### 思路

- 拿到题的直觉解法
    - 二分的题，目标是将题目匹配到模板；
    - 没匹配，就没想清楚，做出来也没意义；下次还是错；
    - 二分需要sort，但是题目没有sort，因此考虑对每个位置的元素，sort后找到合适的插入位置，然后求解；
    - 这里的二分模版应该是用的是：`寻找最右插入位置`或者`寻找最左符合条件(大于x的值)`，如果要用符合条件模版，因为不是相等target关系，所以要改模版，因此选择最右插入位置；
    - 不过在这题里，这个思路是不对的，因为这样的复杂度O(N * NlogN)，完全没必要。BF才只有O(N^2)；
    - 思路有问题，复杂度高是因为每次都要对subarray做sort。因此需要找到一个办法可以节约这部分。

`TLE`

- 使用一个treemap，从左到右，慢慢往里面添加元素，元素是排序好的
- 每次添加的那个即为已知条件里的`j`，已经添加进去的即为可选择的`i`
- 因此对于循环体内的部分，题目的思路调整为:`在有序序列中寻找最右插入位置`，套用模版即可
- 这样也就是在直觉解法的基础上节约出了sort的复杂度；

`AC`


### 代码
```java
//寻找最右插入位置
lass Solution {
    public int solve(int[] nums) {
        int res = 0;
        if(nums.length == 0) return res;
        TreeMap<Integer, Integer> map = new TreeMap<>();
        map.put(nums[0], 1);
        for(int i = 1;i < nums.length;i++){
            List<Integer> keys = new ArrayList<>(map.keySet());
            int boundary = binarySearch(keys, 3 * nums[i]);
            List<Integer> values = new ArrayList<>(map.values());
            for(int j = boundary;j < values.size();j++){
                res += values.get(j);
            }
            map.put(nums[i], map.getOrDefault(nums[i], 0)+1);
        }

        return res;
    }

    public int binarySearch(List<Integer> nums, int target){
        int left = 0, right = nums.size() - 1;
        
        while(left <= right){
            int mid = left + (right - left) / 2;
            if(nums.get(mid) > target){
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```


### 复杂度

time: O(NlogN),遍历N个元素，每个元素需要插入treemap，开销O(logN)，二分，开销O(logN)。总和还是O(NlogN)。
space: O(N)，需要常数个List以及TreeMap。