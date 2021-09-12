### 思路

用array模拟stack的操作。比较特别的是increment这个方法，将栈底的k个元素递增val。

基本的push和pop的操作通过记录index来实现就好。increment这个方法则可以通过`两种`方式.

- 不实用额外空间的基础上，用loop update栈底部的k个值；
- 使用额外的array，存储increment value，然后pop时与原始值求和；
- 在递增array中，变量以"内卷"的方式存储，即只更新第k个值，共所有的前k个值更新所用。这样操作的目的是为了避免循环赋值；


### 代码
```java
//loop for increment
class CustomStack {
    int[] store;
    int top;

    public CustomStack(int maxSize) {
        this.store = new int[maxSize];
        top = -1;
    }
    
    public void push(int x) {
        if(top < store.length - 1){
            store[++top] = x;    
        }
        
    }
    
    public int pop() {
        if(top >= 0){
            return store[top--];
        } else {
            return -1;
        }
        
    }
    
    public void increment(int k, int val) {
        if(top == -1) return;
        int count = k;
        int cur = 0;
        while(count > 0 && cur <= top){
            store[cur] += val;
            count--;
            cur++;
        }
    }
}

//array for increment
class CustomStack {
    int[] store;
    int[] incre;
    int top;

    public CustomStack(int maxSize) {
        this.store = new int[maxSize];
        this.incre = new int[maxSize];
        top = -1;
    }
    
    public void push(int x) {
        if(top < store.length - 1){
            store[++top] = x;    
        }
        
    }
    
    public int pop() {
        if(top < 0) return -1;
        int sum = store[top] + incre[top];
        if(top == 0) {
            incre[top] = 0;
            top--;
            return sum;
        }
        incre[top - 1] += incre[top];
        incre[top] = 0;
        top--;
        return sum;
    }
    
    public void increment(int k, int val) {
        if(top == -1) return;
        if(k > top + 1) k = top + 1;
        incre[k - 1] += val;
    }
}
```

### 复杂度

loop for increment
pop, push复杂度O(1), increment复杂度O(K);
O(1)，不包括用于模拟的array；

Stack
pop, push, increment都是O(1)；
在原本模拟stack的array基础上，额外付出O(N)提供存储incre的array。