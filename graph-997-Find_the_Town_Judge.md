### 思路

- 拿到题的直觉解法
    - 图，在脑袋里根本没形成联系；
    - 这题固然可以不用图(其实思路类似)，但压根没想到outbound/inbound就说不过去了;
    - 两个条件：不能是truster，trustee==n-1
    - 扫一遍，对出现在truster的人，标记为-1，不再考虑；否则递增+1;
    - 最后扫一遍res，== n-1的可以return

`AC`

- 图解法
    - 题目的条件可以转换为indegree与outdegree
    - 如果相信别人,outdegree++
    - 如果被人相信,indegree++
    - 如果indegree-outdegree=n-1，judge
    - 既然是考虑netdegree，干脆只记录它


### 代码
```java
//直觉解法
class Solution {
    public int findJudge(int n, int[][] trust) {
        int[] res = new int[n];
        
        for(int[] pair: trust){
            res[pair[0]-1] = -1;
            if(res[pair[1]-1] != -1){
                res[pair[1]-1] += 1;
            }
        }
        
        for(int i = 0;i < res.length;i++){
            if(res[i] == n-1){
                return i+1;
            }
        }
        return -1;
    }
}

//graph
class Solution {
    public int findJudge(int n, int[][] trust) {
        int[] netbound = new int[n];
        
        for(int i = 0;i < trust.length;i++){
            int truster = trust[i][0]-1;
            int trustee = trust[i][1]-1;
            
            netbound[truster] -= 1;
            netbound[trustee] += 1;
        }
        
        for(int i = 0;i < netbound.length;i++){
            if(netbound[i] == n - 1){
                return i+1;
            }
        }
        return -1;
    }
}
```


### 复杂度

根据题目的两个变量，即edges和vertices
time: O(trust.length)
space: O(n)