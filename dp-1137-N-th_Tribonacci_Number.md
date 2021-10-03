### 思路

- 拿到题的直觉解法
    - 题目直接给你的公式，就很明显提示这是个dp的题；
    - 记忆化递归或者迭代

`AC`

### 代码
```java
//记忆化递归
class Solution {
    HashMap<Integer, Integer> map = new HashMap<>();
    public int tribonacci(int n) {
        map.put(0, 0);
        map.put(1, 1);
        map.put(2, 1);
        return dp(n);
    }
    
    public int dp(int n){
        if(map.containsKey(n)){
            return map.get(n);
        }
        
        int r = tribonacci(n-1) + tribonacci(n-2) + tribonacci(n-3);
        map.put(n, r);
        
        return  r;
    }
}

//记忆结果的迭代
class Solution {
    HashMap<Integer, Integer> map = new HashMap<>();
    public int tribonacci(int n) {
        if(n == 0){
            return 0;
        }
        if(n == 1 || n == 2){
            return 1;
        }
        
        map.put(0, 0);
        map.put(1, 1);
        map.put(2, 1);
        map.put(3, 2);
        int total = 0;
        
        for(int i = 4;i <= n;i++){
            map.put(i, 2 * map.get(i-1) - map.get(i-4));
        }
        return map.get(n);
    }
}

//官网的迭代dp
class Solution {
    public int tribonacci(int n) {
        if(n == 0){
            return 0;
        }
        if(n == 1 || n == 2){
            return 1;
        }
        
        int x = 0;
        int y = 1;
        int z = 1;
        for(int i = 4;i <= n;i++){
            int temp = x + y + z;
            x = y;
            y = z;
            z = temp;
        }
        return x + y + z;
    }
}

```

### 复杂度

//记忆化递归，记忆结果的迭代
time: O(N)，计算N，要计算前面的N-1个。
space: 递归每次不会进入很多层，因为记忆化了，但是存储map需要O(N);

//官网的迭代dp
time: O(N),计算N次
space: 常数O(1),没有用到额外的数据结构
