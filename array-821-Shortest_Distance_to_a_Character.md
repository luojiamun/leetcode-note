### 思路
Array遍历

- 思路与解法
```
- 左右两边各扫一遍；
- 第一遍扫出初始编号；
- 第二遍更新编号找最小值；
```

- 心得
```
- array的题肯定不会只考array，因为是基础；
- array一般肯定要考遍历，就像是binary tree；遍历就是左到右，右到左，左右各扫一遍，这是最基本的，可能再有结合双指针/滑动窗口的；总之就是一些结合题目已知条件需要找到合适的便利方式；
- 这题还可以用stack，边push边更新；
```

### 代码
```java
//array遍历
class Solution {
    public int[] shortestToChar(String s, char c) {
        int[] res = new int[s.length()];
        int ci = 20000;
        Arrays.fill(res, 20000);
        for(int i = 0;i < s.length();i++){
            if(s.charAt(i) == c){
                ci = 0;
            }
            res[i] = Math.min(ci, res[i]);
            ci++;
        }
        
        for(int i = s.length() - 1;i >= 0;i--){
            if(s.charAt(i) == c){
                ci = 0;
            }
            res[i] = Math.min(ci, res[i]);
            ci++;
        }
        
        return res;
      
    }
}

```

### 复杂度

Array遍历
O(s.length());
O(1)
