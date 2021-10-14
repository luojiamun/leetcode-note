### 思路

- 拿到题的直觉解法
    - 有意识的注意写法思路后，流畅多了
    - 双指针，左右缩进


### 代码
```java
//直觉解法
class Solution {
    public int totalFruit(int[] fruits) {
        int res = 0, left = 0;
        HashMap<Integer, Integer> map = new HashMap<>();
        
        for(int i = 0;i < fruits.length;i++){
            int cur = fruits[i];
            map.putIfAbsent(cur, 0);
            map.put(cur, map.get(cur)+1);
            
            while(map.size() > 2){
                int leftFruit = fruits[left];
                map.put(leftFruit, map.get(leftFruit) - 1);
                if(map.get(leftFruit) == 0) map.remove(leftFruit);
                left++;
            }
            res = Math.max(res, i - left + 1);
        }
        return res;
    }
}
```


### 复杂度

time: O(N)
space: O(N)