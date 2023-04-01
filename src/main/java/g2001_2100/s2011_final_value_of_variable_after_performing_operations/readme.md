[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2011\. Final Value of Variable After Performing Operations

Easy

There is a programming language with only **four** operations and **one** variable `X`:

*   `++X` and `X++` **increments** the value of the variable `X` by `1`.
*   `--X` and `X--` **decrements** the value of the variable `X` by `1`.

Initially, the value of `X` is `0`.

Given an array of strings `operations` containing a list of operations, return _the **final** value of_ `X` _after performing all the operations_.

**Example 1:**

**Input:** operations = ["--X","X++","X++"]

**Output:** 1

**Explanation:** The operations are performed as follows:

Initially, X = 0.

--X: X is decremented by 1, X = 0 - 1 = -1.

X++: X is incremented by 1, X = -1 + 1 = 0.

X++: X is incremented by 1, X = 0 + 1 = 1.

**Example 2:**

**Input:** operations = ["++X","++X","X++"]

**Output:** 3

**Explanation:** The operations are performed as follows:

Initially, X = 0.

++X: X is incremented by 1, X = 0 + 1 = 1.

++X: X is incremented by 1, X = 1 + 1 = 2.

X++: X is incremented by 1, X = 2 + 1 = 3.

**Example 3:**

**Input:** operations = ["X++","++X","--X","X--"]

**Output:** 0

**Explanation:** The operations are performed as follows:

Initially, X = 0.

X++: X is incremented by 1, X = 0 + 1 = 1.

++X: X is incremented by 1, X = 1 + 1 = 2.

--X: X is decremented by 1, X = 2 - 1 = 1.

X--: X is decremented by 1, X = 1 - 1 = 0.

**Constraints:**

*   `1 <= operations.length <= 100`
*   `operations[i]` will be either `"++X"`, `"X++"`, `"--X"`, or `"X--"`.

## Solution

```java
public class Solution {
    public int finalValueAfterOperations(String[] operations) {
        int xValue = 0;
        for (String word : operations) {
            if (word.contains("+")) {
                xValue++;
            } else {
                xValue--;
            }
        }
        return xValue;
    }
}
```