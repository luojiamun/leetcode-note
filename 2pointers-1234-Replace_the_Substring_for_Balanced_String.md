### 思路

- 拿到题的直觉解法
    - 有意识的注意写法思路后，流畅多了
    - 双指针，左右缩进


### 代码
```java
class Solution {
    public int balancedString(String s) {
        int res = Integer.MAX_VALUE;
        
        int[] budget = new int[4];
        
        Arrays.fill(budget, s.length() / -4);
        
        for(int i = 0;i < s.length();i++){
            char cur = s.charAt(i);
            if(cur == 'Q') budget[0]++;
            if(cur == 'W') budget[1]++;
            if(cur == 'E') budget[2]++;
            if(cur == 'R') budget[3]++;
        }
        
        if(check(budget)) return 0;
        int left = 0;
        
        
        for(int i = 0;i < s.length();i++){
            char cur = s.charAt(i);
            
            if(cur == 'Q') budget[0]--;
            if(cur == 'W') budget[1]--;
            if(cur == 'E') budget[2]--;
            if(cur == 'R') budget[3]--;
            
            while(check(budget)){
                res = Math.min(res, i - left + 1);
                char leftChar = s.charAt(left);
                if(leftChar == 'Q') budget[0]++;
                if(leftChar == 'W') budget[1]++;
                if(leftChar == 'E') budget[2]++;
                if(leftChar == 'R') budget[3]++;
                left++;
            }
        }
        return res;
    }
    
    public boolean check(int[] budget){
        for(int i: budget){
            if(i > 0) return false;
        }
        return true;
    }
}
```


### 复杂度

time: O(N)
space: O(1)