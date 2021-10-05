### 思路

- 拿到题的直觉解法
    - 时间复杂度O(NlogN)：扫一遍nums，把重复的标记为10000，然后排序一遍；
    - 看答案最优解
    - 无需额外空间时间，扫一遍，边扫边更新；
    - 原理：
        - 一个指针负责写，一个负责扫
        - 判断条件：当前元素和前任是否相同
        - 如果相同，扫的前进，写的不动；
        - 如果不同，写入nums[扫的当前位置]，双指针分别前进；
    - 这个题里，写的指针是显示的，扫的指针其实就是for loop，比较隐式

`AC`


### 代码
```java
//time O(NlogN)
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length == 0) return 0;
        int size = 0, cur = nums[0];
        for(int i = 1;i < nums.length;i++){
          if(nums[i] == cur){
            nums[i] = 10000;
            size++;
          } else {
            cur = nums[i];
          }

        }

        Arrays.sort(nums);

        return nums.length - size;
    }
}

//READER pointer + WRITER pointer;no extra time & space
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length == 0) return 0;
        int pw = 1;
        for(int i = 1;i < nums.length;i++){
          if(nums[i] == nums[i-1]){
            continue;
          } else {
            nums[pw] = nums[i];
            pw++;
          }
        }
        return pw;
    }
}
```


### 复杂度

//要排序的话
time: O(NlogN)
space: O(1)

//读写双指针
time: O(N)
space: O(1)