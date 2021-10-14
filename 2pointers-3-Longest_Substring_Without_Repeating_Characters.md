### 思路

- 拿到题的直觉解法
    - 双指针
    - 左：左边界
    - 右：for循环指针/右边界
    - 左右缩进
    - 每当出现重复时，左指针移到重复的那个字符右边，即：将重复排除在外；

`AC`


### 代码
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int max = 0;
        int left = 0;
        HashMap<Character, Integer> map = new HashMap<>();
        for(int i = 0;i < s.length();i++){
            char cur = s.charAt(i);
            if(map.containsKey(cur) && map.get(cur) >= left){
                int dupIndex = map.get(cur);
                left = dupIndex+1;
            } else {
                map.putIfAbsent(cur, 0);
                map.put(cur, map.get(cur)+1);
            }
            
            map.put(cur, i);
            
            max = Math.max(max, i - left + 1);
        }
        
        return max;
    }
}
```


### 复杂度

time: O(N)
space: O(N)
