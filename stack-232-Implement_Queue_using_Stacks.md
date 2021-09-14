### 思路

借助两个栈，一个input-stack用于接收push，一个output-stack用于pop；
- 所有的push都进入input；
- 所有的pop都出自output；
- 如果output空了，就从input提取；


### 代码
```java
/*
1. init two stacks: input and output
2. when push, push to the input
3. when pop, pop from output, if output is empty, pop from input to output, then pop from output
*/

class MyQueue {
    Stack<Integer> in;
    Stack<Integer> out;
    /** Initialize your data structure here. */
    public MyQueue() {
        this.in = new Stack<>();
        this.out = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        in.add(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if(!out.isEmpty()){
            return out.pop();
        } else if(!in.isEmpty()){
            while(!in.isEmpty()){
                out.add(in.pop());
            }
            return out.pop();
        } else {
            return Integer.MIN_VALUE;
        }
    }
    
    /** Get the front element. */
    public int peek() {
        if(!out.isEmpty()){
            return out.peek();
        } else if(!in.isEmpty()){
            while(!in.isEmpty()){
                out.add(in.pop());
            }
            return out.peek();
        } else {
            return Integer.MIN_VALUE;
        }
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return in.isEmpty() && out.isEmpty();
    }
}
```

### 复杂度

push: O(1);
pop/peek: 如果output-stack站内有元素，O(1)，如果没有，O(M)，M是当前input站内的元素数量；
isEmpty: O(1)

space: O(N),需要维护两个栈，如果这两个栈算是计划内，那就为O(1)，即除栈外只需常数空间。
