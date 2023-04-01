[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 895\. Maximum Frequency Stack

Hard

Design a stack-like data structure to push elements to the stack and pop the most frequent element from the stack.

Implement the `FreqStack` class:

*   `FreqStack()` constructs an empty frequency stack.
*   `void push(int val)` pushes an integer `val` onto the top of the stack.
*   `int pop()` removes and returns the most frequent element in the stack.
    *   If there is a tie for the most frequent element, the element closest to the stack's top is removed and returned.

**Example 1:**

**Input**

["FreqStack", "push", "push", "push", "push", "push", "push", "pop", "pop", "pop", "pop"]

[[], [5], [7], [5], [7], [4], [5], [], [], [], []]

**Output:** [null, null, null, null, null, null, null, 5, 7, 5, 4]

**Explanation:**

    FreqStack freqStack = new FreqStack();
    freqStack.push(5); // The stack is [5]
    freqStack.push(7); // The stack is [5,7]
    freqStack.push(5); // The stack is [5,7,5]
    freqStack.push(7); // The stack is [5,7,5,7]
    freqStack.push(4); // The stack is [5,7,5,7,4]
    freqStack.push(5); // The stack is [5,7,5,7,4,5]
    freqStack.pop(); // return 5, as 5 is the most frequent. The stack becomes [5,7,5,7,4].
    freqStack.pop(); // return 7, as 5 and 7 is the most frequent, but 7 is closest to the top. The stack becomes [5,7,5,4].
    freqStack.pop(); // return 5, as 5 is the most frequent. The stack becomes [5,7,4].
    freqStack.pop(); // return 4, as 4, 5 and 7 is the most frequent, but 4 is closest to the top. The stack becomes [5,7]. 

**Constraints:**

*   <code>0 <= val <= 10<sup>9</sup></code>
*   At most <code>2 * 10<sup>4</sup></code> calls will be made to `push` and `pop`.
*   It is guaranteed that there will be at least one element in the stack before calling `pop`.

## Solution

```java
import java.util.HashMap;

public class FreqStack {
    private static class Node {
        Node next;
        int val;

        Node(int val) {
            this.val = val;
            this.next = null;
        }

        Node() {
            this.next = null;
        }
    }

    private static class DLL {
        DLL next;
        int val;
        Node head;
        int size;

        public DLL() {
            head = new Node();
            this.size = 0;
        }

        public void addNode(int x) {
            Node node = new Node(x);
            node.next = head.next;
            head.next = node;
            size++;
        }

        public Node removeNode() {
            Node node = head.next;
            if (node != null) {
                head.next = node.next;
                node.next = null;
                size--;
            }
            return node;
        }
    }

    private int max;
    private HashMap<Integer, Integer> freqMap;
    private HashMap<Integer, DLL> freqListMap;

    public FreqStack() {
        max = 0;
        freqMap = new HashMap<>();
        freqListMap = new HashMap<>();
    }

    public void push(int val) {
        int count = freqMap.getOrDefault(val, 0) + 1;
        max = Math.max(max, count);
        freqMap.put(val, count);
        DLL dll = freqListMap.getOrDefault(count, new DLL());
        dll.addNode(val);
        freqListMap.put(count, dll);
    }

    public int pop() {
        DLL dll = freqListMap.get(max);
        Node node = dll.removeNode();
        freqMap.put(node.val, max - 1);
        if (dll.size == 0) {
            max--;
        }
        return node.val;
    }
}

/*
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack obj = new FreqStack();
 * obj.push(val);
 * int param_2 = obj.pop();
 */
```