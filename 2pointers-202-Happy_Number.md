### 思路

- 拿到题的直觉解法
    - 直接递归求结果，用hashset追踪，出现循环就退出；

`AC`

- 答案最优解
    - 依旧是用linkedlist探测循环的方法

### 代码
```java
//set
class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> set = new HashSet<>();
        return happify(n, set) == 1;
    }
    
    static int happify(int n, HashSet<Integer> set){
        if(n == 1) return 1;
        int nn = 0;
        while(n > 0){
          nn += (n % 10) * (n % 10);
          n = n / 10;
        }
        if(set.contains(nn)) return -1;
        set.add(nn);
        return happify(nn, set);
  }
}

//linkedlist找循环方法
class Solution {
    public boolean isHappy(int n) {
        int slow = n, fast = n;
        
        while(fast != 1){
            slow = next(slow);
            fast = next(next(fast));
            if(slow == fast){
                break;
            }
        }
        
        return fast == 1;
        
        
    }
    
    public int next(int n){
        int ne = 0;
        while(n > 0){
            ne += (n % 10) * (n % 10);
            n = n / 10;
        }
        return ne;
    }
}
```


### 复杂度

//set
time: O(logN)
space: O(logN)

//双指针
time: O(N)
space: O(1)