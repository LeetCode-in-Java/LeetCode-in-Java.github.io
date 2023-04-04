[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 440\. K-th Smallest in Lexicographical Order

Hard

Given two integers `n` and `k`, return _the_ <code>k<sup>th</sup></code> _lexicographically smallest integer in the range_ `[1, n]`.

**Example 1:**

**Input:** n = 13, k = 2

**Output:** 10

**Explanation:** The lexicographical order is [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9], so the second smallest number is 10. 

**Example 2:**

**Input:** n = 1, k = 1

**Output:** 1 

**Constraints:**

*   <code>1 <= k <= n <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int findKthNumber(int n, int k) {
        int curr = 1;
        k = k - 1;
        while (k > 0) {
            int steps = calSteps(n, curr, curr + 1L);
            if (steps <= k) {
                curr += 1;
                k -= steps;
            } else {
                curr *= 10;
                k -= 1;
            }
        }
        return curr;
    }

    // use long in case of overflow
    private int calSteps(int n, long n1, long n2) {
        long steps = 0;
        while (n1 <= n) {
            steps += Math.min(n + 1L, n2) - n1;
            n1 *= 10;
            n2 *= 10;
        }
        return (int) steps;
    }
}
```