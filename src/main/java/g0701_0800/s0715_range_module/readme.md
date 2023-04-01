[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 715\. Range Module

Hard

A Range Module is a module that tracks ranges of numbers. Design a data structure to track the ranges represented as **half-open intervals** and query about them.

A **half-open interval** `[left, right)` denotes all the real numbers `x` where `left <= x < right`.

Implement the `RangeModule` class:

*   `RangeModule()` Initializes the object of the data structure.
*   `void addRange(int left, int right)` Adds the **half-open interval** `[left, right)`, tracking every real number in that interval. Adding an interval that partially overlaps with currently tracked numbers should add any numbers in the interval `[left, right)` that are not already tracked.
*   `boolean queryRange(int left, int right)` Returns `true` if every real number in the interval `[left, right)` is currently being tracked, and `false` otherwise.
*   `void removeRange(int left, int right)` Stops tracking every real number currently being tracked in the **half-open interval** `[left, right)`.

**Example 1:**

**Input** 

["RangeModule", "addRange", "removeRange", "queryRange", "queryRange", "queryRange"] 

[[], [10, 20], [14, 16], [10, 14], [13, 15], [16, 17]]

**Output:** [null, null, null, true, false, true]

**Explanation:** 

    RangeModule rangeModule = new RangeModule();
    rangeModule.addRange(10, 20); 
    rangeModule.removeRange(14, 16); 
    rangeModule.queryRange(10, 14); // return True,(Every number in [10, 14) is being tracked) 
    rangeModule.queryRange(13, 15); // return False,(Numbers like 14, 14.03, 14.17 in [13, 15) are not being tracked) 
    rangeModule.queryRange(16, 17); // return True, (The number 16 in [16, 17) is still being tracked, despite the remove operation)

**Constraints:**

*   <code>1 <= left < right <= 10<sup>9</sup></code>
*   At most <code>10<sup>4</sup></code> calls will be made to `addRange`, `queryRange`, and `removeRange`.

## Solution

```java
public class RangeModule {
    private final Interval head;

    public RangeModule() {
        head = new Interval(0, 0);
    }

    public void addRange(int left, int right) {
        Interval pos = head;
        while (pos.next != null && pos.next.end < left) {
            pos = pos.next;
        }
        while (pos.next != null && pos.next.start <= right) {
            left = Math.min(pos.next.start, left);
            right = Math.max(pos.next.end, right);
            removeNext(pos);
        }
        insert(pos, new Interval(left, right));
    }

    public boolean queryRange(int left, int right) {
        Interval pos = head;
        while (pos != null) {
            if (left >= pos.start && right <= pos.end) {
                return true;
            }
            pos = pos.next;
        }
        return false;
    }

    public void removeRange(int left, int right) {
        Interval pos = head;
        while (pos.next != null && pos.next.end <= left) {
            pos = pos.next;
        }
        Interval prev = pos;
        Interval curr = pos.next;
        while (curr != null && curr.start < right) {
            if (curr.start < left) {
                insert(prev, new Interval(curr.start, left));
                curr.start = left;
                prev = prev.next;
                curr = prev.next;
                continue;
            }
            if (right >= curr.end) {
                removeNext(prev);
                curr = prev.next;
            } else {
                curr.start = right;
                curr = curr.next;
            }
        }
    }

    private void insert(Interval curr, Interval next) {
        next.next = curr.next;
        curr.next = next;
    }

    private void removeNext(Interval curr) {
        Interval del = curr.next;
        if (del != null) {
            curr.next = del.next;
            del.next = null;
        }
    }

    static class Interval {
        int start;
        int end;
        Interval next;

        public Interval(int l, int r) {
            start = l;
            end = r;
        }
    }
}
```