### 思路

- 拿到题的直觉解法
    - 没做过这种邻接矩阵的类型，拿到题想了半天int[][] graph,这个构成是什么，原来可能是这种{{0}, {}, {2,3}}
    - graph的row长度是固定的，为顶点个数，每个row的长度是不固定的，取决于有没有edge
    - 题目的思路是直接从target出发来bfs，那么能第一个循环回来的，也就是最短的cycle length了
    - 注意需要标注visited，即要排除非target的循环

`AC`


### 代码
```java
import java.util.*;

class Solution {
    public int solve(int[][] graph, int target) {
        int res = 0;
        if(graph[target].length == 0) return -1;
        Queue<Integer> q = new LinkedList<>();
        q.add(target);
        int[] visited = new int[graph.length];
        while(!q.isEmpty()){
            int size = q.size();
            res++;
            for(int i = 0;i < size;i++){
                int cur = q.poll();
                int[] neighbors = graph[cur];
                for(int neighbor: neighbors){
                    if(neighbor == target) return res;
                    if(visited[neighbor]++ > 0) continue;
                    visited[neighbor]++;
                    q.add(neighbor);
                }
            }
        }
        return -1;
    }
}
```


### 复杂度

time: O(V+E)
space: O(V)
