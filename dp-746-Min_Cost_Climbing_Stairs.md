### 思路

- 拿到题的直觉解法
    - dp没练过，每次都要好好练一下思路；
    - 关键是找到dp函数，一种递推关系；
    - 可以top-down,也可以bottom-up
    - 如果从下往上想，将函数F(i)设定为从该位置登顶所需的成本，那么F(i)=cost[i]+Math.min(F(i+1),F(i+2))，这是一个递归关系，因此有了解法1；
    - 那如果从上往下想，结合走楼梯的基本问题leetcode70，登顶前，要么在cost.length - 1,要么在cost.length - 2，如果我们把到达这两个位置的成本定义为F(cost.length - 1)和F(cost.length - 2)，则登顶F(cost.length) = Math.min(cost[cost.length-1]+F(cost.length - 1), cost[cost.length-2]+F(cost.length - 2))；我们可以发现，这是一个递推关系，每一个位置都可以由前两步的信息得到。于是我们有了解法2；

`AC`


### 代码
```java
//从上往下想，top-down，递归
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        HashMap<Integer, Integer> map = new HashMap<>();
        return dp(cost, -1, map);
    }
    
    public int dp(int[] cost, int step, HashMap<Integer, Integer> map){
        if(step >= cost.length) return 0;
        if(map.containsKey(step)) return map.get(step);
        
        int res = (step < 0?0:cost[step]) + Math.min(dp(cost, step+1, map), dp(cost, step+2, map));
        map.put(step, res);
        return res;
    }
}

//从下往上想，bottom-up，递推
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int[] dp = new int[cost.length + 1];
        
        for(int i = 2;i < dp.length;i++){
            int oneStepToHere = dp[i - 1] + cost[i - 1];
            int twoStepToHere = dp[i - 2] + cost[i - 2];
            
            //here
            dp[i] = Math.min(oneStepToHere, twoStepToHere);
        }
        
        return dp[dp.length - 1];
    }
}
```


### 复杂度

time: O(N)，遍历每个step
space: O(N)，都需要，无论是stack，map还是dp[]
