### 思路
Deque, Array遍历；

注意题目要求，1 <= A.length <= 10000，所以直接做加法是不可能的。

### 代码
```java
class Solution {
    public List<Integer> addToArrayForm(int[] num, int k) {
        int pos = 0;
        int carry = 0;
        Deque<Integer> res = new LinkedList<>();
        while(pos < num.length || k >= Math.pow(10, pos) || carry > 0){
            int fromNum = pos < num.length?num[num.length - 1 - pos]:0;
            int fromK = (k / (int)Math.pow(10, pos)) % 10;
            int sum = fromNum + fromK + carry;
            res.addFirst(sum % 10);
            carry = sum > 9?1:0;
            pos++;
        }
                
        return new ArrayList<>(res);
    }
}
```

### 复杂度

k因为是常数，其长度可以忽略。

O(num.length)
O(1)