### 思路

- 拿到题的直觉解法
    - bit也是弱项要多练；
    - XOR对相同的数会为0；不同的数不为零；题目里只有两个不同的数，如果全部来一遍，最后结果即为这两个不同的数XOR；即已知条件1:xor = x + y
    - x和y既然不同，那么在某个bit位一定是不同的，在这个位置上xor的值为1，使用`<<=`操作，找到这个不同的位置：已知条件2；
    - 根据这两个已知条件，遍历一遍原数组；将数组分两组做XOR：一组在`[已知条件2]`的位置的bit为0，另一组在该位置的bit为1。
    - x和y会分别位于这两个组，而其他的元素因为是两两相等的，因此会两两加入某一组，XOR后为0；两个组最后分别只剩下了x和y；

`AC`


### 代码
```java
class Solution {
  public int[] singleNumber(int[] nums) {
    // difference between two numbers (x and y) which were seen only once
    int bitmask = 0;
    for (int num : nums) bitmask ^= num;

    // rightmost 1-bit diff between x and y
    int diff = 1;
    while((bitmask & diff) == 0){
        diff <<= 1;
    }

    int x = 0;
    // bitmask which will contain only x
    for (int num : nums) if ((num & diff) != 0) x ^= num;

    return new int[]{x, bitmask^x};
  }
}
```


### 复杂度

time: O(N)
space: O(1)