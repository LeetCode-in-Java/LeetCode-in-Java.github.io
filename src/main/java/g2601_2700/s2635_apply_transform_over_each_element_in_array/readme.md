[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2635\. Apply Transform Over Each Element in Array

Easy

Given an integer array `arr` and a mapping function `fn`, return a new array with a transformation applied to each element.

The returned array should be created such that `returnedArray[i] = fn(arr[i], i)`.

Please solve it without the built-in `Array.map` method.

**Example 1:**

**Input:** arr = [1,2,3], fn = function plusone(n) { return n + 1; }

**Output:** [2,3,4]

**Explanation:** 

const newArray = map(arr, plusone); // [2,3,4] 

The function increases each value in the array by one.

**Example 2:**

**Input:** arr = [1,2,3], fn = function plusI(n, i) { return n + i; }

**Output:** [1,3,5]

**Explanation:** The function increases each value by the index it resides in.

**Example 3:**

**Input:** arr = [10,20,30], fn = function constant() { return 42; }

**Output:** [42,42,42]

**Explanation:** The function always returns 42.

**Constraints:**

*   `0 <= arr.length <= 1000`
*   <code>-10<sup>9</sup> <= arr[i] <= 10<sup>9</sup></code>
*   `fn returns a number`

## Solution

```typescript
function map(arr: number[], fn: (n: number, i: number) => number): number[] {
    const res: number[] = []
    for (let i = 0; i < arr.length; i++) {
        res.push(fn(arr[i], i))
    }
    return res
}

export { map }
```