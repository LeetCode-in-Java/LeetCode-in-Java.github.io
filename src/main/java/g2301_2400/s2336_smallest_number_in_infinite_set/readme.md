[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2336\. Smallest Number in Infinite Set

Medium

You have a set which contains all positive integers `[1, 2, 3, 4, 5, ...]`.

Implement the `SmallestInfiniteSet` class:

*   `SmallestInfiniteSet()` Initializes the **SmallestInfiniteSet** object to contain **all** positive integers.
*   `int popSmallest()` **Removes** and returns the smallest integer contained in the infinite set.
*   `void addBack(int num)` **Adds** a positive integer `num` back into the infinite set, if it is **not** already in the infinite set.

**Example 1:**

**Input**

["SmallestInfiniteSet", "addBack", "popSmallest", "popSmallest", "popSmallest", "addBack", "popSmallest", "popSmallest", "popSmallest"]

[[], [2], [], [], [], [1], [], [], []]

**Output:**

[null, null, 1, 2, 3, null, 1, 4, 5]

**Explanation:**

    SmallestInfiniteSet smallestInfiniteSet = new SmallestInfiniteSet();
    smallestInfiniteSet.addBack(2); // 2 is already in the set, so no change is made.
    smallestInfiniteSet.popSmallest(); // return 1, since 1 is the smallest number, and remove it from the set.
    smallestInfiniteSet.popSmallest(); // return 2, and remove it from the set.
    smallestInfiniteSet.popSmallest(); // return 3, and remove it from the set.
    smallestInfiniteSet.addBack(1); // 1 is added back to the set.
    smallestInfiniteSet.popSmallest(); // return 1, since 1 was added back to the set and
                                       // is the smallest number, and remove it from the set.
    smallestInfiniteSet.popSmallest(); // return 4, and remove it from the set.
    smallestInfiniteSet.popSmallest(); // return 5, and remove it from the set. 

**Constraints:**

*   `1 <= num <= 1000`
*   At most `1000` calls will be made **in total** to `popSmallest` and `addBack`.

## Solution

```java
public class SmallestInfiniteSet {
    private int[] set = new int[1001];
    private int ptr = 1;

    public SmallestInfiniteSet() {
        for (int i = 1; i <= 1000; i++) {
            set[i] = 1;
        }
    }

    public int popSmallest() {
        int val = -1;
        if (ptr > 0 && ptr <= 1000) {
            set[ptr] = 0;
            val = ptr++;
            while (ptr <= 1000 && set[ptr] == 0) {
                ptr++;
            }
        }
        return val;
    }

    public void addBack(int num) {
        if (set[num] == 0) {
            set[num] = 1;
            if (num < ptr) {
                ptr = num;
            }
        }
    }
}

/*
 * Your SmallestInfiniteSet object will be instantiated and called as such:
 * SmallestInfiniteSet obj = new SmallestInfiniteSet();
 * int param_1 = obj.popSmallest();
 * obj.addBack(num);
 */
```