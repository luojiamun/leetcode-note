### 思路

- 拿到题的直觉解法
    - topK，比较容易联想到heap
    - 首先sort一下array
    - 然后通过滚动的方式计算count，放入heap
    - 最后poll出k个存入结果
    
`AC`

### 代码
```java
//heap
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Arrays.sort(nums);
        
        int count = 1;
        
        PriorityQueue<int[]> heap = new PriorityQueue<>((o1, o2)->o2[1] - o1[1]);
        for(int i = 0;i < nums.length;i++){
            if(i < nums.length - 1 && nums[i] == nums[i+1]){
                count++;
            } else {
                heap.add(new int[]{nums[i], count});
                count = 1;
            }
        }
        int[] res = new int[k];
        
        for(int i = 0;i < k;i++){
            res[i] = heap.poll()[0];
        }
        return res;
    }
}

//hashmap+ArrayList<Integer>[]
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        ArrayList<Integer>[] sums = new ArrayList[nums.length];
        
        //val -> count
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int num: nums){
            map.putIfAbsent(num, 0);
            map.put(num, map.get(num)+1);
        }
        
        for(int key: map.keySet()){
            if(sums[map.get(key) - 1] == null) sums[map.get(key) - 1] = new ArrayList<>();
            sums[map.get(key) - 1].add(key);
        }
        int[] res = new int[k];

        for(int i = sums.length-1;i >= 0;i--){
            List<Integer> lst = sums[i];
            while(lst != null && k-1 >= 0 && !lst.isEmpty()){
                res[k-1] = lst.get(lst.size() - 1);
                lst.remove(lst.size() - 1);
                k--;
            }
            if(k-1 < 0) break;
        }
        return res;
    }
}
```

### 复杂度

//heap
time: 排序,O(NlogN)，遍历一次求count，O(N)，弹出topK,O(KlogD), D是不同的元素的个数，最坏为O(KlogN);总的复杂度取最高O(NlogN);
space：heap所需的空间O(D), D是不同的元素的个数，最坏为O(N)

//hashmap+ArrayList<Integer>[]
time: 没有用到排序，O(N)
space: O(N),比起上一种方法，多用了array of list，但并未影响最终复杂度；