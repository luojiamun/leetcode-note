### 思路

- 拿到题的直觉解法
    - 可以dfs，也可以bfs，遍历在这里不是难点，难点在于，收集到的nodes信息要排序，因为题目要求从上到下，从大到小返回；
    - 可以建立一个tuple存储row,col,value信息，先收集起来。
    - 在遍历过程中，通过heap收集tuple，heap的排序按照col
    - dfs完成后，再来一次循环，对每个col中的值，用list收集，并用comparator分别按row和value排序后，返回最终结果；

`AC`

- 改进
    - 先收集再排序有点繁琐，且需要多次循环；最好能边收集边排序
    - 这里我们的需求是：sorted map，而不只是heap单点排序，所以TreeMap浮出水面
    - TreeMap嵌套两层，分别记录col和row，最后加一层heap对value排序
    - 通过TreeMap，遍历过程中按照col then row的顺序放入map，并将value存入heap
    - 最后按照两层for循环即可。

`AC`

### 代码
```java
//heap
class Solution {
    public List<List<Integer>> verticalTraversal(TreeNode root) {
        
        PriorityQueue<Tuple> nodes = new PriorityQueue<>((t1, t2)->t1.col - t2.col);
        
        preorder(root, nodes, 0, 0);
        
        List<List<Integer>> result = new ArrayList<>();
        List<Tuple> colNodes = new ArrayList<>();
        int curCol = nodes.peek().col;
        while(!nodes.isEmpty()){
            if(nodes.peek().col != curCol){
                Collections.sort(colNodes, Comparator.<Tuple>comparingInt(o->o.row).thenComparing(o->o.value));
                List<Integer> r = new ArrayList<>();
                for(Tuple t: colNodes){
                    r.add(t.value);
                }
                result.add(new ArrayList<>(r));
                colNodes.clear();
                curCol = nodes.peek().col;
            } else {
                Tuple cur = nodes.poll();
                
                colNodes.add(cur);
            }
        }
        Collections.sort(colNodes, Comparator.<Tuple>comparingInt(o->o.row).thenComparing(o->o.value));
        List<Integer> r = new ArrayList<>();
        for(Tuple t: colNodes){
            r.add(t.value);
        }
        result.add(new ArrayList<>(r));
        
        return result;
    }
    
    public PriorityQueue<Tuple> preorder(TreeNode root, PriorityQueue<Tuple> nodes, int row, int col){
        if(root == null) {
            return nodes;
        }
        
        Tuple rootT = new Tuple(row, col, root.val);
        nodes.add(rootT);
        
        preorder(root.left, nodes, row+1, col-1);
        preorder(root.right, nodes, row+1, col+1);
        
        return nodes;
    }
    
    
    private class Tuple{
        int row;
        int col;
        int value;
        public Tuple(int row, int col, int value){
            this.row = row;
            this.col = col;
            this.value = value;
        }
    }
}

//TreeMap + heap

class Solution {
    public List<List<Integer>> verticalTraversal(TreeNode root) {
        TreeMap<Integer, TreeMap<Integer, PriorityQueue<Integer>>> map = new TreeMap<>();
        
        preorder(root, map, 0, 0);
        
        List<List<Integer>> result = new ArrayList<>();
        for(int col: map.keySet()){
            List<Integer> curCol = new ArrayList<>();
            
            for(int row: map.get(col).keySet()){
                PriorityQueue<Integer> pq = map.get(col).get(row);
                while(!pq.isEmpty()){
                    curCol.add(pq.poll());
                }
            }
            result.add(curCol);
        }
        
        return result;
    }
    
    public TreeMap<Integer, TreeMap<Integer, PriorityQueue<Integer>>> preorder(TreeNode root, TreeMap<Integer, TreeMap<Integer, PriorityQueue<Integer>>> map, int row, int col){
        if(root == null) {
            return map;
        }
        
        map.putIfAbsent(col, new TreeMap<>());
        map.get(col).putIfAbsent(row, new PriorityQueue<>());
        map.get(col).get(row).add(root.val);
        
        preorder(root.left, map, row+1, col-1);
        preorder(root.right, map, row+1, col+1);
        
        return map;
    }
}
```

### 复杂度

time: preorder需要遍历每个node，O(N);外部循环需要在遍历每个node，每个O(logN),总共O(NlogN)；因此综合来说O(NlogN)
space: 需要额外的空间存储heap/map，O(N)