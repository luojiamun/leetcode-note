### 思路

- 拿到题的直觉解法
    - sort和heap一起使用，按照要求先sort；
    - 每一轮，将符合时间范围的放入heap，然后弹出top存入res
    - 重点是处理边界情况：如果这轮结束了，pool空了，怎么办？需要将时间范围扩大到下一个task；

`AC`

- 思路有，一写就一塌糊涂。这题写了超过2小时，才能把边界问题处理好，但也处理的很难看；

### 代码
```java
class Solution {
    public int[] getOrder(int[][] tasks) {
        int[] res = new int[tasks.length];
        int[][] copy = new int[tasks.length][tasks[0].length+1];
        for(int i = 0;i < tasks.length;i++){
            copy[i][0] = tasks[i][0];
            copy[i][1] = tasks[i][1];
            copy[i][2] = i;
        }
        Arrays.sort(copy, Comparator.<int[]>comparingInt(o -> o[0]));
        PriorityQueue<int[]> heap = new PriorityQueue<>(Comparator.<int[]>comparingInt(o -> o[1]).thenComparingInt(o -> o[2]));
        int time = 0;
        int taskIndex = 0;
        int resIndex = 0;
        
        while(resIndex < res.length){
            while(taskIndex < tasks.length && copy[taskIndex][0] <= time){
                heap.add(new int[]{copy[taskIndex][0], copy[taskIndex][1], copy[taskIndex][2]});
                taskIndex++;
            }
            if(heap.isEmpty()){
                time = copy[taskIndex][0];
                continue;
            }
            
            int[] top = heap.poll();
            res[resIndex++] = top[2];
            time += top[1];      
        }
        
        return res;
    }
}
```


### 复杂度

time: O(NlogN)，因为sort了。
space: O(N)，有辅助array和heap。
