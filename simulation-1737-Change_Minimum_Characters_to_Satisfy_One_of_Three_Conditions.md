### 思路

- 参考答案的解法
    - 难度在于读懂题，然后怎么把条件翻译成代码
    - 第三个条件：有点歧义，答案要求把a和b变成一样的；读起来像是各自一样即可；
    - 第一和第二条件：因为只有26个字母，我们可以省略复杂的判断条件，无视a与b目前的字母组成；直接遍历26个字母，假设我们要把a和b改成对应的字母，需要的步数；
    - 频繁使用：总数-不变的=变化的；
    - 思路转换很重要。

`AC`

### 代码
```java
class Solution {
    public int minCharacters(String a, String b) {
        int min = Integer.MAX_VALUE;
        int[] counterA = new int[26];
        int[] counterB = new int[26];
        
        for(int i = 0;i < a.length();i++){
            counterA[a.charAt(i) - 'a'] += 1;
        }
        for(int i = 0;i < b.length();i++){
            counterB[b.charAt(i) - 'a'] += 1;
        }
        
        //opt 3
        for(int i = 0;i < 26;i++){
            min = Math.min(min, a.length() + b.length() - counterA[i] - counterB[i]);
        }
        
        for(int i = 1;i < 26;i++){
            int changed = 0;
            for(int j = i;j < 26;j++){
                changed += counterA[j];
            }
            for(int j = 0;j < i;j++){
                changed += counterB[j];
            }
            min = Math.min(min, changed);
        }
        
        
        for(int i = 1;i < 26;i++){
            int changed = 0;
            for(int j = i;j < 26;j++){
                changed += counterB[j];
            }
            for(int j = 0;j < i;j++){
                changed += counterA[j];
            }
            min = Math.min(min, changed);
        }
        
        
        return min;
        
    }
}
```


### 复杂度

time: O(N)
space: O(1)
