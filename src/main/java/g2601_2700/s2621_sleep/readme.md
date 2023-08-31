[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2621\. Sleep

Easy

Given a positive integer `millis`, write an asynchronous function that sleeps for `millis` milliseconds. It can resolve any value.

**Example 1:**

**Input:** millis = 100

**Output:** 100

**Explanation:** It should return a promise that resolves after 100ms. let t = Date.now(); sleep(100).then(() => { console.log(Date.now() - t); // 100 });

**Example 2:**

**Input:** millis = 200

**Output:** 200

**Explanation:** It should return a promise that resolves after 200ms.

**Constraints:**

*   `1 <= millis <= 1000`

## Solution

```typescript
async function sleep(millis: number): Promise<void> {
    await new Promise((resolve) => setTimeout(resolve, millis))
}

/*
 * let t = Date.now()
 * sleep(100).then(() => console.log(Date.now() - t)) // 100
 */

export { sleep }
```