### 思路

- 拿到题的直觉解法
    - 双指针固定窗口k，然后滑动向右，检查头尾的元音情况；

`AC`


### 代码
```java
class Solution {
    public int maxVowels(String s, int k) {
        int count = 0;
        int max = 0;
        var set = Set.of('a', 'e', 'i', 'o', 'u');
        
        while(count < k){
            if(set.contains(s.charAt(count++))){
                max++;
            }
        }
        int last = max;
        int right = k;
        for(int i = 0;i < s.length()-k;i++){
            int c1 = set.contains(s.charAt(i))?-1:0;
            int c2 = set.contains(s.charAt(right++))?1:0;
            last = last + c1 + c2;
            max = Math.max(max, last);
        }
        
        return max;
    }
}
```


### 复杂度

time: O(N)
space: O(1)
