[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1362\. Closest Divisors

Medium

Given an integer `num`, find the closest two integers in absolute difference whose product equals `num + 1` or `num + 2`.

Return the two integers in any order.

**Example 1:**

**Input:** num = 8

**Output:** [3,3]

**Explanation:** For num + 1 = 9, the closest divisors are 3 & 3, for num + 2 = 10, the closest divisors are 2 & 5, hence 3 & 3 is chosen.

**Example 2:**

**Input:** num = 123

**Output:** [5,25]

**Example 3:**

**Input:** num = 999

**Output:** [40,25]

**Constraints:**

*   `1 <= num <= 10^9`

## Solution

```java
public class Solution {
    public int[] closestDivisors(int num) {
        int sqrt1 = (int) Math.sqrt(num + 1.0);
        int sqrt2 = (int) Math.sqrt(num + 2.0);
        if (sqrt1 * sqrt1 == num + 1) {
            return new int[] {sqrt1, sqrt1};
        }
        if (sqrt2 * sqrt2 == num + 2) {
            return new int[] {sqrt2, sqrt2};
        }
        int[] ans1 = new int[2];
        for (int i = sqrt1; i >= 1; i--) {
            if ((num + 1) % i == 0) {
                ans1 = new int[] {i, (num + 1) / i};
                break;
            }
        }
        int[] ans2 = new int[2];
        for (int i = sqrt2; i >= 1; i--) {
            if ((num + 2) % i == 0) {
                ans2 = new int[] {i, (num + 2) / i};
                break;
            }
        }
        return Math.abs(ans2[0] - ans2[1]) < Math.abs(ans1[0] - ans1[1]) ? ans2 : ans1;
    }
}
```