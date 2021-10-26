### 思路

- 拿到题的直觉解法
    - 可以直接套用双指针模版，for套while，比较map即可，这样的方式思路最清楚，用来锻炼模版很好，但是复杂度有点高；
    - 可以进一步优化一下map的比较使得复杂度降低，但需要修改模版

`AC`


### 代码
```java
//模版直接套用
class Solution {
    public String minWindow(String s, String t) {
        int left = 0;
        int[] res = {0, Integer.MAX_VALUE};
        HashMap<Character, Integer> map = new HashMap<>();
        HashMap<Character, Integer> source = new HashMap<>();
        
        for(int i = 0;i < t.length();i++){
            char cur = t.charAt(i);
            source.putIfAbsent(cur, 0);
            source.put(cur, source.get(cur)+1);
        }
        
        for(int i = 0;i < s.length();i++){
            char cur = s.charAt(i);
            map.putIfAbsent(cur, 0);
            map.put(cur, map.get(cur)+1);
            
            while(included(map, source)){
                if(i - left < res[1] - res[0]){
                    res[0] = left;
                    res[1] = i;
                }
                char leftChar = s.charAt(left);
                left++;
                map.put(leftChar, map.get(leftChar)-1);
            }
        }
        
        return res[1] == Integer.MAX_VALUE?"":s.substring(res[0], res[1]+1);
    }
    
    public boolean included(HashMap<Character, Integer> map, HashMap<Character, Integer> source){
        for(char key: source.keySet()){
            if(!map.containsKey(key) || map.get(key) < source.get(key)){
                return false;
            }
        }
        
        return true;
    }
}
//优化比较map
class Solution {
    public String minWindow(String s, String t) {
        int left = 0, budget = 0;
        int[] res = {0, Integer.MAX_VALUE};
        HashMap<Character, Integer> map = new HashMap<>();
        
        for(int i = 0;i < t.length();i++){
            char cur = t.charAt(i);
            map.putIfAbsent(cur, 0);
            map.put(cur, map.get(cur)+1);
            budget++;
        }
        
        for(int i = 0;i < s.length();i++){
            char cur = s.charAt(i);
            if(map.containsKey(cur)){
                int latest = map.get(cur);
                if(latest > 0) budget--;
                map.put(cur, map.get(cur) - 1);
            }
            
            while(budget <= 0){
                if(i - left < res[1] - res[0]){
                    res[0] = left;
                    res[1] = i;
                }
                char leftC = s.charAt(left);
                if(map.containsKey(leftC)){
                    map.put(leftC, map.get(leftC) + 1);
                    if(map.get(leftC) > 0) budget++;
                }
                left++;
            }
        }
        
        return res[1] == Integer.MAX_VALUE?"":s.substring(res[0], res[1]+1);
    }
    
}
```


### 复杂度

//直接套用模版
time: O(mn)
space: O(m+n)

//优化
time: O(n+m)
space:O(n)