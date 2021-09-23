### 思路

- 拿到题的直觉解法
    1. if len == 1, return ""
    2. if from 0 to len/2, char - 'a' > 0, char to 'a', if not found, last one +1; odd len, then len/2 will be ignored

string的题都很恶心。。

`AC`

### 代码
```java
class Solution {
    public String breakPalindrome(String palindrome) {
        if(palindrome.length() <= 1) return "";
        
        char[] chars = palindrome.toCharArray();
        boolean found = false;
        for(int i = 0;i <= chars.length / 2;i++){
            if(chars.length % 2 != 0 && i == chars.length / 2){
                continue;
            }
            if(chars[i] - 'a' > 0){
                found = true;
                chars[i] = 'a';
                break;
            }
        }
        if(!found){
           chars[chars.length-1] += 1;
        }
        StringBuilder sb = new StringBuilder();
        sb.append(chars);
        return sb.toString();
    }
}
```

### 复杂度

time: O(N)
space: 维护char[],O(N);