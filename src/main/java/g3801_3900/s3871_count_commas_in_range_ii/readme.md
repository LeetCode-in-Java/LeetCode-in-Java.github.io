[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3871\. Count Commas in Range II

Medium

You are given an integer `n`.

Return the **total** number of commas used when writing all integers from `[1, n]` (inclusive) in **standard** number formatting.

In **standard** formatting:

*   A comma is inserted after **every three** digits from the right.
*   Numbers with **fewer** than 4 digits contain no commas.

**Example 1:**

**Input:** n = 1002

**Output:** 3

**Explanation:**

The numbers `"1,000"`, `"1,001"`, and `"1,002"` each contain one comma, giving a total of 3.

**Example 2:**

**Input:** n = 998

**Output:** 0

**Explanation:**

All numbers from 1 to 998 have fewer than four digits. Therefore, no commas are used.

**Constraints:**

*   <code>1 <= n <= 10<sup>15</sup></code>

## Solution

```java
public class Solution {
    public long countCommas(long n) {
        long count = 0;
        if (n >= 1000L) {
            count += n - 999L;
        }
        if (n >= 1000000L) {
            count += n - 999999L;
        }
        if (n >= 1000000000L) {
            count += n - 999999999L;
        }
        if (n >= 1000000000000L) {
            count += n - 999999999999L;
        }
        if (n >= 1000000000000000L) {
            count += n - 999999999999999L;
        }
        return count;
    }
}
```