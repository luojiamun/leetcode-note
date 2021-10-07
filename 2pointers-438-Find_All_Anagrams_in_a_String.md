### 思路

- 拿到题的直觉解法
    - 题目不难，但是实现很慢，说明写代码不熟；
    - 和帖子里描述的一样，看起来会，一旦写起来有各种点，发现都没考虑到，不停地卡住；
    - 这是写少了，经验不足的体现；
    - 此题bf可以通过，每次都用窗口中的和p做暴力匹配
    - 优化的解法用到map和双指针


`AC`


### 代码
```java
//暴力解
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        char[] pc = p.toCharArray();
        Arrays.sort(pc);
        List<Integer> res = new ArrayList<>();
        HashMap<Character, Integer> map = new HashMap<>();
        for(char c: p.toCharArray()){
            map.put(c, 1);
        }
        int left = 0;
        for(int i = 0;i < s.length() - p.length() + 1;i++){
            if(!map.containsKey(s.charAt(i)) || !map.containsKey(s.charAt(i +p.length() - 1)) ) {
                left = i;
                continue;
            }
            char[] subs = s.substring(i, i+p.length()).toCharArray();
            if(check(subs, pc)) res.add(i);
        }
        return res;
    }
    
    public boolean check(char[] s, char[] p){
        Arrays.sort(s);
        
        for(int i = 0;i < s.length;i++){
            if(s[i] != p[i]){
                return false;
            }
        }
        return true;
    }
}

//优化解
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        HashMap<Character, Integer> mapP = new HashMap<>();
        
        for(char c: p.toCharArray()){
            mapP.putIfAbsent(c, 0);
            mapP.put(c, mapP.get(c)+1);
        }
        
        HashMap<Character, Integer> mapS = new HashMap<>();
        List<Integer> res = new ArrayList<>();
        for(int i = 0;i < s.length();i++){
            char cur = s.charAt(i);
            
            mapS.putIfAbsent(cur, 0);
            mapS.put(cur, mapS.get(cur)+1);
            
            if(i >= p.length()) {
                int first = i - p.length();
                char firstChar = s.charAt(first);
                
                mapS.put(firstChar, mapS.get(firstChar)-1);
                if(mapS.get(firstChar) == 0) mapS.remove(firstChar);
            }
            
            if(mapS.equals(mapP)) res.add(i - p.length() + 1);
        }
        return res;
            
    }
}
```


### 复杂度

//暴力解
time: S * PlogP
space: O(S)

//优化解
time: O(S)假设S的长度远大于P
space: O(P)