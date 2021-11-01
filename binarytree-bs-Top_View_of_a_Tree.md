### 思路

- 拿到题的直觉解法
    - 和Leetcode987几乎一样，算是变体吧。可以dfs，也可以bfs，遍历在这里不是难点，难点在于，收集到的nodes信息要排序，因为题目要求从左到右(右边被忽略)，从上到下返回(下面被忽略)；
    - 按照987的思路，利用TreeMap可以做到边收集，边过滤，边排序；
    - 过滤，是因为题目要求从左到右的优先级；那么采取先来后过滤的原则，采取preorder，已经是从左到右了，[row][col]如果已经有人了，那后来的就会被忽略。

`AC`

### 代码
```java
import java.util.*;

class Solution {
    public int[] solve(Tree root) {
        TreeMap<Integer, TreeMap<Integer, Integer>> map = new TreeMap<>();

        preorder(root, 0, 0, map);

        int[] res = new int[map.size()];
        int i = 0;

        for(int col: map.keySet()){
            int row = map.get(col).firstKey();
            res[i++] = map.get(col).get(row);
        }
        return res;

    }

    public TreeMap<Integer, TreeMap<Integer, Integer>> preorder(Tree root, int row, int col, TreeMap<Integer, TreeMap<Integer, Integer>> map){
        if(root == null) return map;

        if(!map.containsKey(col)){
            TreeMap<Integer, Integer> temp = new TreeMap<>();
            map.put(col, temp);
        }
        //忽略右边的，也就是后来的；
        if(!map.get(col).containsKey(row)){
            map.get(col).put(row, root.val);
        }

        preorder(root.left, row+1, col-1, map);
        preorder(root.right, row+1, col+1, map);

        return map;
    }
}
```

### 复杂度

time: preorder需要遍历每个node，每次TreeMap操作需要logN，O(NlogN);
space: 需要额外的空间存储map，O(N)