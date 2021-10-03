### 思路

- 拿到题的直觉解法
    - 用两个指针，left,right，从起点出发；
    - 如果没有遇到重复的，right++，将值和index存入map
    - 如果遇到了重复的，且重复的在左指针右边，将左指针挪到右指针所在位置，更新map
    - 循环的每一步都更新长度max

`AC`

### 代码
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashMap<Character, Integer> map = new HashMap<>();
        
        int left = 0, max = Integer.MIN_VALUE;
        
        for(int right = 0;right < s.length();right++){
            char c = s.charAt(right);
            if(map.containsKey(c) && map.get(c) >= left){
                left = map.get(c) + 1;
                map.put(c, right);
            } else {
                map.put(c, right);
            }
            max = Math.max(max, right - left + 1);
        }
        return max == Integer.MIN_VALUE?0:max;
    }
}
```

### 复杂度

time: O(N);
space：map所需的空间O(N)