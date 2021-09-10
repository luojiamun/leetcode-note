### 思路
Array遍历

两次遍历
- 第一次从左到右，遇到c，将其右边的依次从1-m编号；直到遇到下一个c，重复编号过程。（第一个c左边为未编号状态）
- 第二次从右到左，遇到c，将其左边当前编号与预计编号比较并取min；第二次编号并会将第一个c左边的编号完成。

Stack
将s遍历一次
- 如果当前为c，
    - pop站内元素，并依次按距离递增编号；直到栈空或栈顶为c；当前位置编号0；将当前位置压入栈
- 如果当前不为c
    - 如果栈顶为c，那么当前位置编号1
    - 否则当前位置编号为前一个位置编号+1；
    - 将当前位置index 压入栈；


### 代码
```java
//array遍历
class Solution {
    public int[] shortestToChar(String s, char c) {
        int pre = Integer.MIN_VALUE / 2;
        int[] res = new int[s.length()];
        for(int i = 0;i < s.length();i++){
            if(s.charAt(i) == c) pre = i;
            res[i] = i - pre;
        }
        
        pre = Integer.MAX_VALUE;
        for(int i = s.length() - 1;i >= 0;i--){
            if(s.charAt(i) == c) pre = i;
            res[i] = Math.min(res[i], pre - i);
        }
        
        return res;
    }
}

//stack
class Solution {
    public int[] shortestToChar(String s, char c) {
        Stack<Integer> stack = new Stack<>();
        int[] res = new int[s.length()];
        stack.add(0);
        for(int i = 0;i < s.length();i++){
            if(s.charAt(i) == c){
                int pos = 0;
                while(!stack.isEmpty() && s.charAt(stack.peek()) != c){
                    int cur = stack.pop();
                    
                    res[cur] = res[cur] == 0?++pos:Math.min(++pos, res[cur]);
                }
                res[i] = 0;
                stack.add(i);
            } else {
                if(!stack.isEmpty() && s.charAt(stack.peek()) == c){
                    res[i] = 1;
                } else if(i > 0 && res[i-1] != 0) {
                    res[i] = res[i-1] + 1;
                }
                stack.add(i);
            }
        }
        
        return res;
    }
}
```

### 复杂度

Array遍历
O(s.length());
O(1)

Stack
O(s.length())
O(N),维护一个栈。