[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4000\. Largest Integer With Given Digit Sum

Easy

You are given two non-negative integers `n` and `s`.

Return the **largest** integer that has **at most** `n` digits and whose sum of digits is `s`. If no such integer exists, return -1.

OriginalJava**Example 1:**

**Input:** n = 2, s = 9

**Output:** 90

**Explanation:**

The largest integer with at most 2 digits that has a sum of digits of 9 is 90.

**Example 2:**

**Input:** n = 2, s = 19

**Output:** \-1

**Explanation:**

There is no integer with at most 2 digits that has a sum of digits of 19, so the answer is -1.

**Example 3:**

**Input:** n = 5, s = 0

**Output:** 0

**Explanation:**

The only non-negative integer whose digits sum to 0 is 0.

**Constraints:**

*   `1 <= n <= 5`
*   `0 <= s <= 100`

## Solution

```java
public class Solution {
    public int largestInteger(int n, int s) {
        if (9 * n < s) {
            return -1;
        }
        if (s == 0) {
            return 0;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            int d = Math.min(9, s);
            sb.append(d);
            s -= d;
        }
        return Integer.parseInt(sb.toString());
    }
}
```