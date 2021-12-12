### 思路

- 拿到题的直觉解法
    - 暴力backtrack
    - 

`AC`


### 代码
```java
//backtrack
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> res = new ArrayList<>();
        if(digits.isEmpty()) return res;
        
        backtrack(digits, new StringBuilder(), 0, res);
        
        return res;
    }
    
    public List<String> backtrack(String digits, StringBuilder sb, int index, List<String> res){
        if(index == digits.length()){
            res.add(sb.toString());
            return res;
        }
        Map<Character, String> map = Map.of(
        '2', "abc", '3', "def", '4', "ghi", '5', "jkl", 
        '6', "mno", '7', "pqrs", '8', "tuv", '9', "wxyz");
        
        String chars = map.get(digits.charAt(index));
        
        for(int i = 0;i < chars.length();i++){
            char c = chars.charAt(i);
            sb.append(c);
            backtrack(digits, sb, index+1, res);
            sb.delete(sb.toString().length() - 1, sb.toString().length());
        }
        return res;
        
    }
}


```


### 复杂度

//backtrack
time: O(N*4^N)
space: O(N)
