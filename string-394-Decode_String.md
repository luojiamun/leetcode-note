### 思路

stack, string

这题的主要考点

- 利用栈，在遇到右括号`]`时pop进行运算，运算完成后push回到stack；
    - 这个技巧可以说用的蛮多，类似的技巧在1381也用到过；
    - `字符` * `数字` = `新字符`；这里字符和数字可以分别存在两个栈，也可以存在一个栈，判断条件不同；

需要注意的地方

- 因为用的是栈，所以每次pop之后，得到的String顺序是反的，数字也是反的，得reverse；
- 注意每次数字部分都需要reverse再做repeat操作；但字符只需要最后对res做reverse操作，如果在循环体就对字符做reverse，比较容易混淆；
- String操作，要避免`str = str + "abc"`或`str = "abc" + str`这类操作，因为它们是O(N)的；虽然每次操作的N不是很大，但最坏的情况可以和s.length()一样，最终导致O(N^2);
- 用StringBuilder.append来做，是O(1)的。而且可以很方便的使用reverse()方法。




### 代码
```java
class Solution {
    public String decodeString(String s) {
        Stack<String> stack = new Stack<>();
        
        for(int i = 0;i < s.length();i++){
            if(s.charAt(i) == ']'){
                //get chars inside []
                StringBuilder s_sb = new StringBuilder();
                while(!stack.isEmpty() && !stack.peek().equals("[")){
                    s_sb.append(stack.pop());
                }
                stack.pop();//for '['
                
                //get numeric multiplier
                StringBuilder n_sb = new StringBuilder();
                while(!stack.isEmpty() && stack.peek().charAt(0) <= '9' && stack.peek().charAt(0) >= '0'){
                    n_sb.append(stack.pop());
                }
                
                stack.add(s_sb.toString().repeat(Integer.valueOf(n_sb.reverse().toString())));
            } else {
                stack.add(String.valueOf(s.charAt(i)));
            }
        }
        
        StringBuilder resb = new StringBuilder();
        while(!stack.isEmpty()){
            resb.append(stack.pop());
        }
        return resb.reverse().toString();
    }
}
```

### 复杂度

O(N)，循环体会从头到尾遍历s.length();
O(N)，要维护一个栈；
