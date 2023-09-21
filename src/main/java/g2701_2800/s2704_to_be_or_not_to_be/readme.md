[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2704\. To Be Or Not To Be

Easy

Write a function `expect` that helps developers test their code. It should take in any value `val` and return an object with the following two functions.

*   `toBe(val)` accepts another value and returns `true` if the two values `===` each other. If they are not equal, it should throw an error `"Not Equal"`.
*   `notToBe(val)` accepts another value and returns `true` if the two values `!==` each other. If they are equal, it should throw an error `"Equal"`.

**Example 1:**

**Input:** func = () => expect(5).toBe(5)

**Output:** {"value": true}

**Explanation:** 5 === 5 so this expression returns true.

**Example 2:**

**Input:** func = () => expect(5).toBe(null)

**Output:** {"error": "Not Equal"}

**Explanation:** 5 !== null so this expression throw the error "Not Equal".

**Example 3:**

**Input:** func = () => expect(5).notToBe(null)

**Output:** {"value": true}

**Explanation:** 5 !== null so this expression returns true.

## Solution

```typescript
type ToBeOrNotToBe = {
    toBe: (val: any) => boolean
    notToBe: (val: any) => boolean
}

const expect = (val: any): ToBeOrNotToBe => ({
    toBe: (equality: any) => {
        if (val !== equality) {
            throw new Error('Not Equal')
        }
        return true
    },
    notToBe: (equality: any) => {
        if (val === equality) {
            throw new Error('Equal')
        }
        return true
    },
})

/*
 * expect(5).toBe(5); // true
 * expect(5).notToBe(5); // throws "Equal"
 */

export { expect }
```