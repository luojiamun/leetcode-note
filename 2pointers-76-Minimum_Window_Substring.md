### 思路

- 拿到题的直觉解法
    - 这个题，看起来很简单，但写起来很繁琐；
    - 主要的思路是，快慢双指针；
    - 快指针负责遍历，慢指针负责缩进；
    - 一个特殊的时点是第一次找到所有覆盖t的序列；
    - 在此之后就是边走边尝试缩进；
    - 主要是通过hashmap记录char的预算，多退少补；
    - 一旦是`找到`的状态，则只有右边有补的，左边才会缩进

`AC`


### 代码
```java
class Solution {
    public String minWindow(String s, String t) {
        if(s.equals(t)) return t;
        
        HashMap<Character, Integer> map = new HashMap<>();
        
        for(char c: t.toCharArray()){
            map.putIfAbsent(c, 0);
            map.put(c, map.get(c)+1);
        }
        
        int[] min = new int[2];
        min[0] = -100000;
        min[1] = 100000;
        Deque<Tuple> dq = new LinkedList<>();
        int count = 0;
        for(int i = 0;i < s.length();i++){
            char curC = s.charAt(i);
            
            if(map.containsKey(curC)){
                Tuple tp = new Tuple(curC, i);
                dq.add(tp);
                if(map.get(curC) > 0) count++;
                map.put(curC, map.get(curC) - 1);
            }

            if(count == t.length()){
                while(map.get(dq.peekFirst().c) < 0){
                    Tuple dup = dq.pollFirst();
                    map.put(dup.c, map.get(dup.c)+1);
                }
                if(dq.peekLast().i - dq.peekFirst().i < min[1] - min[0]){
                    min[1] = dq.peekLast().i;
                    min[0] = dq.peekFirst().i;
                }
            }
        }
        if(count < t.length()) return "";
        
        StringBuilder sb = new StringBuilder();
        
        for(int i = min[0];i <= min[1];i++){
            sb.append(s.charAt(i));
        }
        return sb.toString();
    }
    
    private class Tuple{
        char c;
        int i;
        
        public Tuple(char c, int i){
            this.c = c;
            this.i = i;
        }
    }
}
```


### 复杂度

//双指针
time: O(长度和)
space: O(T长度)