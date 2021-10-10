### 思路

- 拿到题的直觉解法
    - 因为只是按照指令走，指令不可重复，因此只要模拟就可以了。

`AC`

### 代码
```java
class Solution {
    public boolean judgeCircle(String moves) {
        int[] directions = new int[4];
        
        for(int i = 0;i < moves.length();i++){
            char cur = moves.charAt(i);
            switch (cur) {
                case 'U': 
                    directions[0]++;
                    break;
                case 'R': 
                    directions[1]++;
                    break;
                case 'D': 
                    directions[2]++;
                    break;
                default:
                    directions[3]++;
            }
        }
        
        return directions[0] == directions[2] && directions[1] == directions[3];
    }
}
```


### 复杂度

time: O(N)
space: O(1)，有限个元素的array依旧是常数级
