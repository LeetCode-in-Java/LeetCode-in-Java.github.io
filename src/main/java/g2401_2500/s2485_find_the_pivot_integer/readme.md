[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2485\. Find the Pivot Integer

Easy

Given a positive integer `n`, find the **pivot integer** `x` such that:

*   The sum of all elements between `1` and `x` inclusively equals the sum of all elements between `x` and `n` inclusively.

Return _the pivot integer_ `x`. If no such integer exists, return `-1`. It is guaranteed that there will be at most one pivot index for the given input.

**Example 1:**

**Input:** n = 8

**Output:** 6

**Explanation:** 6 is the pivot integer since: 1 + 2 + 3 + 4 + 5 + 6 = 6 + 7 + 8 = 21.

**Example 2:**

**Input:** n = 1

**Output:** 1

**Explanation:** 1 is the pivot integer since: 1 = 1.

**Example 3:**

**Input:** n = 4

**Output:** -1

**Explanation:** It can be proved that no such integer exist.

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```java
public class Solution {
    public int pivotInteger(int n) {
        if (n == 0 || n == 1) {
            return n;
        }
        int sum = 0;
        for (int i = 1; i <= n; i++) {
            sum += i;
        }
        int ad = 0;
        for (int i = 1; i <= n; i++) {
            ad += i - 1;
            sum -= i;
            if (sum == ad) {
                return i;
            }
        }
        return -1;
    }
}
```