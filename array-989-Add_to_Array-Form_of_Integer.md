### 思路

- 思路与解法
```
- 逐位加；
- 反向存，最后reverse，不然正向存，每次add数组都得挪位；
```

- 心得
```
- 第一次fail了因为大意了，这题要注意num和k的长短不一，不能一个for on num解决；
```

### 代码
```java
class Solution {
    public List<Integer> addToArrayForm(int[] num, int k) {
        int carry = 0, i = num.length - 1;
        List<Integer> res = new ArrayList<>();
        
        while(i >= 0 || k > 0 || carry > 0){
            int left = i >= 0?num[i]:0;
            i--;
            int right = k % 10;
            k = k / 10;
            int sum = left + right + carry;
            res.add(sum % 10);
            carry = sum / 10;
        }

        Collections.reverse(res);
        return res;
    }
}
```

### 复杂度

k因为是常数，其长度可以忽略。

time: O(Math.max(num.length, Math.log10(k)+1))
space: O(1)，不算结果