[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2622\. Cache With Time Limit

Medium

Write a class that allows getting and setting key-value pairs, however a **time until expiration** is associated with each key.

The class has three public methods:

`set(key, value, duration)`: accepts an integer `key`, an integer `value`, and a `duration` in milliseconds. Once the `duration` has elapsed, the key should be inaccessible. The method should return `true` if the same un-expired key already exists and `false` otherwise. Both the value and duration should be overwritten if the key already exists.

`get(key)`: if an un-expired key exists, it should return the associated value. Otherwise it should return `-1`.

`count()`: returns the count of un-expired keys.

**Example 1:**

**Input:** ["TimeLimitedCache", "set", "get", "count", "get"] [[], [1, 42, 100], [1], [], [1]] [0, 0, 50, 50, 150]

**Output:** [null, false, 42, 1, -1]

**Explanation:** 

At t=0, the cache is constructed. 

At t=0, a key-value pair (1: 42) is added with a time limit of 100ms. The value doesn't exist so false is returned. 

At t=50, key=1 is requested and the value of 42 is returned. 

At t=50, count() is called and there is one active key in the cache. 

At t=100, key=1 expires. 

At t=150, get(1) is called but -1 is returned because the cache is empty.

**Example 2:**

**Input:** ["TimeLimitedCache", "set", "set", "get", "get", "get", "count"] [[], [1, 42, 50], [1, 50, 100], [1], [1], [1], []] [0, 0, 40, 50, 120, 200, 250]

**Output:** [null, false, true, 50, 50, -1]

**Explanation:** 

At t=0, the cache is constructed. 

At t=0, a key-value pair (1: 42) is added with a time limit of 50ms. The value doesn't exist so false is returned. 

At t=40, a key-value pair (1: 50) is added with a time limit of 100ms. A non-expired value already existed so true is returned and the old value was overwritten. 

At t=50, get(1) is called which returned 50. At t=120, get(1) is called which returned 50. 

At t=140, key=1 expires. At t=200, get(1) is called but the cache is empty so -1 is returned. 

At t=250, count() returns 0 because the cache is empty.

**Constraints:**

*   <code>0 <= key <= 10<sup>9</sup></code>
*   <code>0 <= value <= 10<sup>9</sup></code>
*   `0 <= duration <= 1000`
*   `total method calls will not exceed 100`

## Solution

```typescript
class TimeLimitedCache {
    private readonly keyMap: Map<number, any>
    constructor() {
        this.keyMap = new Map<number, any>()
    }

    set(key: number, value: number, duration: number): boolean {
        let existed: boolean = this.keyMap.has(key)
        if (existed) clearTimeout(this.keyMap.get(key).clearRef)

        this.keyMap.set(key, {
            value,
            clearRef: setTimeout(() => {
                this.keyMap.delete(key)
            }, duration),
        })
        return existed
    }

    get(key: number): number {
        return this.keyMap.has(key) ? this.keyMap.get(key).value : -1
    }

    count(): number {
        return this.keyMap.size
    }
}

/*
 * Your TimeLimitedCache object will be instantiated and called as such:
 * var obj = new TimeLimitedCache()
 * obj.set(1, 42, 1000); // false
 * obj.get(1) // 42
 * obj.count() // 1
 */

export { TimeLimitedCache }
```