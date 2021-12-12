### 思路

- 拿到题的直觉解法
    - 每个node都可以当作root-断点，从而考虑其左边和右边的nodes，当作branches
    - 递归下去，左边branch有多少种情况，右边branch有多少种情况，最后会决定当前root的最终unique count
    - 因为左右分步，取乘法；

`AC`


### 代码
```java
class Solution {
    // int total;
    public int numTrees(int n) {
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(1, 1);
        map.put(0, 1);
        return dp(n, map);
    }
    
    public int dp(int n, HashMap<Integer, Integer> map){
        if(map.containsKey(n)) return map.get(n);
        int total = 0;
        for(int i = 1;i <= n;i++){
            int l = dp(i - 1, map);
            map.put(i - 1, l);
            int r = dp(n - i, map);
            map.put(n - i, r);
            total += l * r;
        }
        
        return total;
    }
}
```


### 复杂度

time: O(N^2)
space: O(N)