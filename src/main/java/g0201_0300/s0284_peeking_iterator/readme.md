[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 284\. Peeking Iterator

Medium

Design an iterator that supports the `peek` operation on an existing iterator in addition to the `hasNext` and the `next` operations.

Implement the `PeekingIterator` class:

*   `PeekingIterator(Iterator<int> nums)` Initializes the object with the given integer iterator `iterator`.
*   `int next()` Returns the next element in the array and moves the pointer to the next element.
*   `boolean hasNext()` Returns `true` if there are still elements in the array.
*   `int peek()` Returns the next element in the array **without** moving the pointer.

**Note:** Each language may have a different implementation of the constructor and `Iterator`, but they all support the `int next()` and `boolean hasNext()` functions.

**Example 1:**

**Input**

    ["PeekingIterator", "next", "peek", "next", "next", "hasNext"]
    [[[1, 2, 3]], [], [], [], [], []]

**Output:** [null, 1, 2, 2, 3, false]

**Explanation:**

    PeekingIterator peekingIterator = new PeekingIterator([1, 2, 3]); // [1,2,3]
    peekingIterator.next(); // return 1, the pointer moves to the next element [1,2,3].
    peekingIterator.peek(); // return 2, the pointer does not move [1,2,3].
    peekingIterator.next(); // return 2, the pointer moves to the next element [1,2,3]
    peekingIterator.next(); // return 3, the pointer moves to the next element [1,2,3]
    peekingIterator.hasNext(); // return False 

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `1 <= nums[i] <= 1000`
*   All the calls to `next` and `peek` are valid.
*   At most `1000` calls will be made to `next`, `hasNext`, and `peek`.

**Follow up:** How would you extend your design to be generic and work with all types, not just integer?

## Solution

```java
import java.util.Iterator;

// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html

public class PeekingIterator implements Iterator<Integer> {
    private Iterator<Integer> it;
    private Integer current;

    public PeekingIterator(Iterator<Integer> iterator) {
        // initialize any member here.
        it = iterator;
        current = it.next();
    }

    // Returns the next element in the iteration without advancing the iterator.
    public Integer peek() {
        if (current == null) {
            current = it.next();
        }
        return current;
    }

    // hasNext() and next() should behave the same as in the Iterator interface.
    // Override them if needed.
    @Override
    public Integer next() {
        Integer temp = current;
        current = it.hasNext() ? it.next() : null;
        return temp;
    }

    @Override
    public boolean hasNext() {
        return current != null;
    }
}
```