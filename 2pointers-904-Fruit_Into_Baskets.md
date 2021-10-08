### 思路

- 拿到题的直觉解法
    - 有意识的注意写法思路后，流畅多了
    - 双指针，左右缩进


### 代码
```java
//直觉解法
class Solution {
    public int totalFruit(int[] fruits) {
        HashMap<Integer, Integer> map = new HashMap<>();
        
        int left = 0, max = 0, sum = 0;
            
        for(int i = 0;i < fruits.length;i++){
            int fruit = fruits[i];
            map.putIfAbsent(fruit, 0);
            map.put(fruit, map.get(fruit)+1);
            sum++;
            while(map.size() > 2){
                sum--;
                int leftFruit = fruits[left++];
                if(map.get(leftFruit) > 1){
                    map.put(leftFruit, map.get(leftFruit) - 1);
                } else {
                    map.remove(leftFruit);
                }
            }
            max = Math.max(max, sum);
        }
        return max; 
    }
}
```


### 复杂度

time: O(N)
space: O(N)