[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 225\. Implement Stack using Queues

Easy

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

*   `void push(int x)` Pushes element x to the top of the stack.
*   `int pop()` Removes the element on the top of the stack and returns it.
*   `int top()` Returns the element on the top of the stack.
*   `boolean empty()` Returns `true` if the stack is empty, `false` otherwise.

**Notes:**

*   You must use **only** standard operations of a queue, which means that only `push to back`, `peek/pop from front`, `size` and `is empty` operations are valid.
*   Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.

**Example 1:**

**Input** ["MyStack", "push", "push", "top", "pop", "empty"] [[], [1], [2], [], [], []]

**Output:** [null, null, null, 2, 2, false]

**Explanation:**

    MyStack myStack = new MyStack();
    myStack.push(1);
    myStack.push(2);
    myStack.top(); // return 2
    myStack.pop(); // return 2
    myStack.empty(); // return False 

**Constraints:**

*   `1 <= x <= 9`
*   At most `100` calls will be made to `push`, `pop`, `top`, and `empty`.
*   All the calls to `pop` and `top` are valid.

**Follow-up:** Can you implement the stack using only one queue?

## Solution

```java
import java.util.LinkedList;
import java.util.Queue;

public class MyStack {
    private Queue<Integer> queueOne;
    private Queue<Integer> queueTwo;
    private int top;

    // Initialize your data structure here.
    public MyStack() {
        queueOne = new LinkedList<>();
        queueTwo = new LinkedList<>();
        top = 0;
    }

    // Push element x onto stack.
    public void push(int x) {
        queueOne.add(x);
        top = x;
    }

    // Removes the element on top of the stack and returns that element.
    public int pop() {
        while (queueOne.size() > 1) {
            int val = queueOne.remove();
            top = val;
            queueTwo.add(val);
        }
        int popValue = queueOne.remove();
        queueOne.addAll(queueTwo);
        queueTwo.clear();
        return popValue;
    }

    // Get the top element.
    public int top() {
        return top;
    }

    // Returns whether the stack is empty.
    public boolean empty() {
        return queueOne.isEmpty();
    }
}
```