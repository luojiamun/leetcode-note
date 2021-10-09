### 思路

- 拿到题的直觉解法
    - 以为可以探测是否有环
    - 试了一下发现有环也可以分组，看样子得遍历图

`Failed`

- 答案解法
    - 图的dfs
    - 用邻接矩阵建立图
    - 每一行代表连接到一个node的所有nodes
    - 遍历每个node，如果未分组，drill down
    - 如果已分组，看是否有分组错误

`AC`

- 注意
    - 图的dfs不熟练
    - 图的dfs没有形成思路和模板


### 代码
```java
class Solution {
    public boolean possibleBipartition(int n, int[][] dislikes) {
        int[][] graph = new int[n][n];
        
        for(int i = 0;i < dislikes.length;i++){
            int row = dislikes[i][0] - 1;
            int col = dislikes[i][1] - 1;
            graph[row][col] = 1;
            graph[col][row] = 1;
        }
        int[] visited = new int[n];
        for(int i = 0;i < n;i++){
            if(visited[i] == 0 && !dfs(graph, i, visited, 1)){
                return false;
            }
        }
        return true;
    }
    
    public boolean dfs(int[][] graph, int point, int[] visited, int group){
        visited[point] = group;
        //all edges
        for(int i = 0;i < graph[point].length;i++){
            if(graph[point][i] != 0){
                if(visited[i] != 0){
                    if(visited[i] == group){
                        return false;
                    }
                } else {
                    if(!dfs(graph, i, visited, -group)){
                        return false;
                    }
                }
            }
        }
        return true;
    }
}
```


### 复杂度

time: O(V+E)，每个边在建图时都遍历到了，每个点在dfs时都遍历到了。
space: O(V)，建图需要array， O(N)，以及递归需要的空间O(N).递归最深入的情况，比如1-2, 2-3, 3-4, 4-5, 5-6，就有V层了；