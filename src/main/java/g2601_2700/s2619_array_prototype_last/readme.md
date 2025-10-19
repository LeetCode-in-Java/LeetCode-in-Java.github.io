[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2619\. Array Prototype Last

Easy

Write code that enhances all arrays such that you can call the `array.last()` method on any array and it will return the last element. If there are no elements in the array, it should return `-1`.

You may assume the array is the output of `JSON.parse`.

**Example 1:**

**Input:** nums = [null, {}, 3]

**Output:** 3

**Explanation:** Calling nums.last() should return the last element: 3.

**Example 2:**

**Input:** nums = []

**Output:** -1

**Explanation:** Because there are no elements, return -1.

**Constraints:**

*   `0 <= arr.length <= 1000`

## Solution

```typescript
declare global {
    interface Array<T> {
        last(): T | -1
    }
}

Array.prototype.last = function () { //NOSONAR
    return this.length !== 0 ? this[this.length - 1] : -1 //NOSONAR
}

/*
 * const arr = [1, 2, 3];
 * arr.last(); // 3
 */

export {} //NOSONAR
```