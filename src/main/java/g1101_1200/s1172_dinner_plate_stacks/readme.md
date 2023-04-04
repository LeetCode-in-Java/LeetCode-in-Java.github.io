[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1172\. Dinner Plate Stacks

Hard

You have an infinite number of stacks arranged in a row and numbered (left to right) from `0`, each of the stacks has the same maximum capacity.

Implement the `DinnerPlates` class:

*   `DinnerPlates(int capacity)` Initializes the object with the maximum capacity of the stacks `capacity`.
*   `void push(int val)` Pushes the given integer `val` into the leftmost stack with a size less than `capacity`.
*   `int pop()` Returns the value at the top of the rightmost non-empty stack and removes it from that stack, and returns `-1` if all the stacks are empty.
*   `int popAtStack(int index)` Returns the value at the top of the stack with the given index `index` and removes it from that stack or returns `-1` if the stack with that given index is empty.

**Example 1:**

**Input** 

["DinnerPlates", "push", "push", "push", "push", "push", "popAtStack", "push", "push", "popAtStack", "popAtStack", "pop", "pop", "pop", "pop", "pop"] 

[[2], [1], [2], [3], [4], [5], [0], [20], [21], [0], [2], [], [], [], [], []]

**Output:** [null, null, null, null, null, null, 2, null, null, 20, 21, 5, 4, 3, 1, -1]

**Explanation:** 

    DinnerPlates D = DinnerPlates(2); // Initialize with capacity = 2
    D.push(1); 
    D.push(2); 
    D.push(3); 
    D.push(4); 
    D.push(5);          // The stacks are now: 2 4 
                                              1 3 5 
                                              ﹈ ﹈ ﹈ 
    D.popAtStack(0);    // Returns 2. The stacks are now:   4 
                                                       1 3 5 
                                                      ﹈ ﹈ ﹈ 
    D.push(20);         // The stacks are now: 20 4 
                                                1 3 5 
                                               ﹈ ﹈ ﹈ 
    D.push(21);         // The stacks are now: 20 4 21 
                                                1 3 5 
                                                ﹈ ﹈ ﹈ 
    D.popAtStack(0);    // Returns 20. The stacks are now: 4 21 
                                                           1 3 5 
                                                          ﹈ ﹈ ﹈ 
    D.popAtStack(2);    // Returns 21. The stacks are now: 4 
                                                         1 3 5 
                                                        ﹈ ﹈ ﹈ 
    D.pop()             // Returns 5. The stacks are now: 4 
                                                         1 3 
                                                         ﹈ ﹈ 
    D.pop()             // Returns 4. The stacks are now: 1 3 
                                                         ﹈ ﹈ 
    D.pop()             // Returns 3. The stacks are now: 1 
                                                          ﹈ 
    D.pop()             // Returns 1. There are no stacks. 
    D.pop()             // Returns -1. There are still no stacks.

**Constraints:**

*   <code>1 <= capacity <= 2 * 10<sup>4</sup></code>
*   <code>1 <= val <= 2 * 10<sup>4</sup></code>
*   <code>0 <= index <= 10<sup>5</sup></code>
*   At most <code>2 * 10<sup>5</sup></code> calls will be made to `push`, `pop`, and `popAtStack`.

## Solution

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;
import java.util.TreeSet;

public class DinnerPlates {
    private final List<Stack<Integer>> stacks;
    private int stackCap;
    private TreeSet<Integer> leftIndex;

    public DinnerPlates(int capacity) {
        stacks = new ArrayList<>();
        stackCap = capacity;
        leftIndex = new TreeSet<>();
    }

    public void push(int val) {
        if (!leftIndex.isEmpty()) {
            int i = leftIndex.first();
            stacks.get(i).push(val);
            if (stacks.get(i).size() == stackCap) {
                leftIndex.remove(i);
            }
            return;
        }
        if (stacks.isEmpty() || stacks.get(stacks.size() - 1).size() == stackCap) {
            Stack<Integer> newStack = new Stack<>();
            stacks.add(newStack);
        }
        stacks.get(stacks.size() - 1).push(val);
    }

    public int pop() {
        if (stacks.isEmpty()) {
            return -1;
        }
        while (stacks.get(stacks.size() - 1).isEmpty()) {
            leftIndex.remove(stacks.size() - 1);
            stacks.remove(stacks.size() - 1);
        }
        int val = stacks.get(stacks.size() - 1).pop();
        if (stacks.get(stacks.size() - 1).isEmpty()) {
            leftIndex.remove(stacks.size() - 1);
            stacks.remove(stacks.size() - 1);
        }
        return val;
    }

    public int popAtStack(int index) {
        if (stacks.size() - 1 >= index) {
            int val = -1;
            if (!stacks.get(index).isEmpty()) {
                val = stacks.get(index).pop();
            }
            if (stacks.get(index).isEmpty() && index == stacks.size() - 1) {
                leftIndex.remove(stacks.size() - 1);
                stacks.remove(index);
                return val;
            }
            leftIndex.add(index);
            return val;
        }
        return -1;
    }
}
```