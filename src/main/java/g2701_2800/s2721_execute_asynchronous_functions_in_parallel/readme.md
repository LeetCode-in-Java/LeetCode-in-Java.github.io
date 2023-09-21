[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2721\. Execute Asynchronous Functions in Parallel

Medium

Given an array of asynchronous functions `functions`, return a new promise `promise`. Each function in the array accepts no arguments and returns a promise.

`promise` resolves:

*   When all the promises returned from `functions` were resolved successfully. The resolved value of `promise` should be an array of all the resolved values of promises in the same order as they were in the `functions`.

`promise` rejects:

*   When any of the promises returned from `functions` were rejected. `promise` should also reject with the reason of the first rejection.

Please solve it without using the built-in `Promise.all` function.

**Example 1:**

**Input:** 

    functions = [ 
        () => new Promise(resolve => setTimeout(() => resolve(5), 200)) 
    ]

**Output:** {"t": 200, "resolved": [5]}

**Explanation:** promiseAll(functions).then(console.log); // [5] The single function was resolved at 200ms with a value of 5.

**Example 2:**

**Input:** 
    
    functions = [ 
        () => new Promise(resolve => setTimeout(() => resolve(1), 200)), 
        () => new Promise((resolve, reject) => setTimeout(() => reject("Error"), 100)) 
    ]

**Output:** {"t": 100, "rejected": "Error"}

**Explanation:** Since one of the promises rejected, the returned promise also rejected with the same error at the same time.

**Example 3:**

**Input:** 
    
    functions = [ 
        () => new Promise(resolve => setTimeout(() => resolve(4), 50)), 
        () => new Promise(resolve => setTimeout(() => resolve(10), 150)), 
        () => new Promise(resolve => setTimeout(() => resolve(16), 100)) 
    ]

**Output:** {"t": 150, "resolved": [4, 10, 16]}

**Explanation:** All the promises resolved with a value. The returned promise resolved when the last promise resolved.

**Constraints:**

*   `functions is an array of functions that returns promises`
*   `1 <= functions.length <= 10`

## Solution

```typescript
async function promiseAll<T>(functions: (() => Promise<T>)[]): Promise<T[]> {
    const resolved = []
    let counter = 0

    return new Promise((resolve, reject) => {
        for (let i = 0; i < functions.length; i++) {
            functions[i]()
                .then((res) => {
                    // must specify index of array
                    resolved[i] = res
                    counter++
                    if (counter === functions.length) {
                        resolve(resolved)
                    }
                })
                .catch((err) => {
                    reject(err)
                })
        }
    })
}

/*
 * const promise = promiseAll([() => new Promise(res => res(42))])
 * promise.then(console.log); // [42]
 */

export { promiseAll }
```