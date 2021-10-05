### 思路

- 拿到题的直觉解法
    - 扫一遍得到各个颜色的count
    - 再扫一遍填入即可
- 优化时间解法
    - 头尾各一个指针，指向0和2的边界；
    - 再加一个reader指针，从左往右扫一遍；
    - 遇到0，和边界指针p0交换；reader指针往前一步，因为交换回来的肯定是1，不可能是2，因为在之前已经扫过了；
    - 遇到2，和边界指针2交换；reader指针原地不动，因为交换回来的还不知道是什么，可能是0；

`AC`


### 代码
```java
//time O(N) space O(1)
class Solution {
    public void sortColors(int[] nums) {
        int red = 0, white = 0, blue = 0;
        
        for(int i: nums){
            switch (i) {
                case 0:
                    red++;
                    break;
                case 1:
                    white++;
                    break;
                default:
                    blue++;
                    break;
            }
        }
        
        for(int i = 0;i < nums.length;i++){
            if(red-- > 0){
                nums[i] = 0;
            } else if(white-- > 0){
                nums[i] = 1;
            } else {
                nums[i] = 2;
            }
        }
        
        return;
    }
}

//双指针
class Solution {
    public void sortColors(int[] nums) {
        int redP = 0, blueP = nums.length-1, curr = 0;
    
        while(curr <= blueP){
          if(nums[curr] == 0){
            int temp = nums[curr];
            nums[curr++] = nums[redP];
            nums[redP++] = temp;
          } else if(nums[curr] == 2){
            int temp = nums[curr];
            nums[curr] = nums[blueP];
            nums[blueP--] = temp;
          } else {
            curr++;
          }
        }
        return;

    }
}
```


### 复杂度

//填空
time: O(N)
space: O(1)

//双指针
time: O(N)
space: O(1)