### 思路

- 拿到题的直觉解法
    - 对每个点，要用hashmap存储计算的距离，如果有相同的就形成一个集合，然后排列组合计算有多少个pair；

需要注意的问题：不要过早优化
花了不少时间去想怎么优化。真正面试时，最好是先按照简单的思路写出来，再边沟通，边优化；
想太多优化，会容易钻进去，缺乏沟通，而且最后可能会卡住，影响思路；
    
`AC`

### 代码
```java
class Solution {
    public int numberOfBoomerangs(int[][] points) {
        if(points.length < 3) return 0;
        int res = 0;
        for(int i = 0;i < points.length;i++){
            int[] sp = points[i];
            HashMap<Long, Integer> distanceToPoint = new HashMap<>();
            for(int j = 0;j < points.length;j++){
                if(i == j) continue;
                long dis = (sp[0] - points[j][0])*(sp[0] - points[j][0]) + (sp[1] - points[j][1])*(sp[1] - points[j][1]);
                if(distanceToPoint.containsKey(dis)){
                    distanceToPoint.put(dis, distanceToPoint.get(dis)+1);
                }else{
                    distanceToPoint.put(dis, 1);
                }
                
            }
            for(long dis: distanceToPoint.keySet()){
                int k = distanceToPoint.get(dis);
                res += k*(k-1);
            }
        }
        return res;
    }
}
```

### 复杂度

time: 每个点都遍历了所有其他点，O(N^2)
space: 需要一个hashmap存储，O(N)
