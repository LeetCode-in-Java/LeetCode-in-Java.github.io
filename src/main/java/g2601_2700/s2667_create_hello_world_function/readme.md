[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2667\. Create Hello World Function

Easy

Write a function `createHelloWorld`. It should return a new function that always returns `"Hello World"`.

**Example 1:**

**Input:** args = []

**Output:** "Hello World"

**Explanation:** 

    const f = createHelloWorld(); 
    f(); // "Hello World" 

The function returned by createHelloWorld should always return "Hello World".

**Example 2:**

**Input:** args = [{},null,42]

**Output:** "Hello World"

**Explanation:** 

    const f = createHelloWorld(); 
    f({}, null, 42); // "Hello World" 

Any arguments could be passed to the function but it should still always return "Hello World".

**Constraints:**

*   `0 <= args.length <= 10`

## Solution

```typescript
const createHelloWorld = () => () => "Hello World";

/*
 * const f = createHelloWorld();
 * f(); // "Hello World"
 */

export { createHelloWorld }
```