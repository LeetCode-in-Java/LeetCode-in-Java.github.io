[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2727\. Is Object Empty

Easy

Given an object or an array, return if it is empty.

*   An empty object contains no key-value pairs.
*   An empty array contains no elements.

You may assume the object or array is the output of `JSON.parse`.

**Example 1:**

**Input:** obj = {"x": 5, "y": 42}

**Output:** false

**Explanation:** The object has 2 key-value pairs so it is not empty.

**Example 2:**

**Input:** obj = {}

**Output:** true

**Explanation:** The object doesn't have any key-value pairs so it is empty.

**Example 3:**

**Input:** obj = [null, false, 0]

**Output:** false

**Explanation:** The array has 3 elements so it is not empty.

**Constraints:**

*    <code>2 <= JSON.stringify(obj).length <= 10<sup>5</sup></code>

**Can you solve it in O(1) time?**

## Solution

```typescript
function isEmpty(obj: Record<string, any> | any[]): boolean {
    return Object.keys(obj).length === 0
}

export { isEmpty }
```