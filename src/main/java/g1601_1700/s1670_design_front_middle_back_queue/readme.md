[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1670\. Design Front Middle Back Queue

Medium

Design a queue that supports `push` and `pop` operations in the front, middle, and back.

Implement the `FrontMiddleBack` class:

*   `FrontMiddleBack()` Initializes the queue.
*   `void pushFront(int val)` Adds `val` to the **front** of the queue.
*   `void pushMiddle(int val)` Adds `val` to the **middle** of the queue.
*   `void pushBack(int val)` Adds `val` to the **back** of the queue.
*   `int popFront()` Removes the **front** element of the queue and returns it. If the queue is empty, return `-1`.
*   `int popMiddle()` Removes the **middle** element of the queue and returns it. If the queue is empty, return `-1`.
*   `int popBack()` Removes the **back** element of the queue and returns it. If the queue is empty, return `-1`.

**Notice** that when there are **two** middle position choices, the operation is performed on the **frontmost** middle position choice. For example:

*   Pushing `6` into the middle of `[1, 2, 3, 4, 5]` results in `[1, 2, 6, 3, 4, 5]`.
*   Popping the middle from `[1, 2, 3, 4, 5, 6]` returns `3` and results in `[1, 2, 4, 5, 6]`.

**Example 1:**

**Input:** ["FrontMiddleBackQueue", "pushFront", "pushBack", "pushMiddle", "pushMiddle", "popFront", "popMiddle", "popMiddle", "popBack", "popFront"]

[[], [1], [2], [3], [4], [], [], [], [], []]

**Output:** [null, null, null, null, null, 1, 3, 4, 2, -1]

**Explanation:**

    FrontMiddleBackQueue q = new FrontMiddleBackQueue(); q.pushFront(1); // [1]
    q.pushBack(2); // [1, 2]
    q.pushMiddle(3); // [1, 3, 2]
    q.pushMiddle(4); // [1, 4, 3, 2]
    q.popFront(); // return 1 -> [4, 3, 2]
    q.popMiddle(); // return 3 -> [4, 2]
    q.popMiddle(); // return 4 -> [2]
    q.popBack(); // return 2 -> []
    q.popFront(); // return -1 -> [] (The queue is empty) 

**Constraints:**

*   <code>1 <= val <= 10<sup>9</sup></code>
*   At most `1000` calls will be made to `pushFront`, `pushMiddle`, `pushBack`, `popFront`, `popMiddle`, and `popBack`.

## Solution

```java
public class FrontMiddleBackQueue {
    private int[] queue = new int[1000];
    private int cur = -1;

    public FrontMiddleBackQueue() {
        // empty constructor
    }

    public void pushFront(int val) {
        cur++;
        for (int i = cur; i > 0; i--) {
            queue[i] = queue[i - 1];
        }
        queue[0] = val;
    }

    public void pushMiddle(int val) {
        if (cur < 0) {
            pushFront(val);
            return;
        }
        cur++;
        int mid = cur / 2;
        for (int i = cur; i > mid; i--) {
            queue[i] = queue[i - 1];
        }
        queue[mid] = val;
    }

    public void pushBack(int val) {
        if (cur < 0) {
            pushFront(val);
            return;
        }
        cur++;
        queue[cur] = val;
    }

    public int popFront() {
        if (cur < 0) {
            return -1;
        }
        int result = queue[0];
        for (int i = 0; i < cur; i++) {
            queue[i] = queue[i + 1];
        }
        cur--;
        return result;
    }

    public int popMiddle() {
        if (cur < 0) {
            return -1;
        }
        int mid = cur / 2;
        int result = queue[mid];
        for (int i = mid; i < cur; i++) {
            queue[i] = queue[i + 1];
        }
        cur--;
        return result;
    }

    public int popBack() {
        if (cur < 0) {
            return -1;
        }
        int result = queue[cur];
        cur--;
        return result;
    }
}

/*
 * Your FrontMiddleBackQueue object will be instantiated and called as such:
 * FrontMiddleBackQueue obj = new FrontMiddleBackQueue();
 * obj.pushFront(val);
 * obj.pushMiddle(val);
 * obj.pushBack(val);
 * int param_4 = obj.popFront();
 * int param_5 = obj.popMiddle();
 * int param_6 = obj.popBack();
 */
```