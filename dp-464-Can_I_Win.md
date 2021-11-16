### 思路

- 首先要知道，什么时候游戏结束了？但凡对手能从剩余的排里选出赢你的，就赢了，因此这里要问的是，一定赢或者一定输的情况。
- 那就干脆是穷举法，或者backtrack，推演每种可能；剪枝部分就是如果某种情况已经输了，那就全局输了；
- 写法上是最好用答案里的bit的操作，最简洁；
- backtrack写法上也是答案里思路最简洁；直接放答案


### 代码
```java
public class Solution {
    public boolean canIWin(int maxChoosableInteger, int desiredTotal) {

        if (maxChoosableInteger >= desiredTotal) return true;
        if ((1 + maxChoosableInteger) * maxChoosableInteger / 2 < desiredTotal) return false;

        Boolean[] dp = new Boolean[(1 << maxChoosableInteger) - 1];
        return dfs(maxChoosableInteger, desiredTotal, 0, dp);
    }

    private boolean dfs(int maxChoosableInteger, int desiredTotal, int state, Boolean[] dp) {
        if (dp[state] != null)
            return dp[state];
        for (int i = 1; i <= maxChoosableInteger; i++){
            int tmp = (1 << (i - 1));
            if ((tmp & state) == 0){
                if (desiredTotal - i <= 0 || !dfs(maxChoosableInteger, desiredTotal - i, tmp|state, dp)) {
                    dp[state] = true;
                    return true;
                }
            }
        }
        dp[state] = false;
        return false;
    }
}
```


### 复杂度

time: O(2^N * N)
space: O(2^N * N)