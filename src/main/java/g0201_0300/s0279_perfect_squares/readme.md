[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 279\. Perfect Squares

Medium

Given an integer `n`, return _the least number of perfect square numbers that sum to_ `n`.

A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, `1`, `4`, `9`, and `16` are perfect squares while `3` and `11` are not.

**Example 1:**

**Input:** n = 12

**Output:** 3

**Explanation:** 12 = 4 + 4 + 4. 

**Example 2:**

**Input:** n = 13

**Output:** 2

**Explanation:** 13 = 4 + 9. 

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    private boolean validSquare(int n) {
        int root = (int) Math.sqrt(n);
        return root * root == n;
    }

    public int numSquares(int n) {
        if (n < 4) {
            return n;
        }
        if (validSquare(n)) {
            return 1;
        }
        while ((n & 3) == 0) {
            n >>= 2;
        }
        if ((n & 7) == 7) {
            return 4;
        }
        int x = (int) Math.sqrt(n);
        for (int i = 1; i <= x; i++) {
            if (validSquare(n - i * i)) {
                return 2;
            }
        }
        return 3;
    }
}
```