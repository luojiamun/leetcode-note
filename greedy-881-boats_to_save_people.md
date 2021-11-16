### 思路

- 拿到题的直觉解法
    - 注意题目条件，每船最多两人；
    - 那就看重的能不能带上轻的了，能带上就带上，带不上就自己一船；

`AC`


### 代码
```java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);
        int left = 0, right = people.length - 1, res = 0;
        while(left <= right){
            res++;
            if(people[left] + people[right] <= limit){
                left++;
            }
            right--;
        }
        return res;
    }
}
```


### 复杂度

time: O(NlogN)
space: O(1)