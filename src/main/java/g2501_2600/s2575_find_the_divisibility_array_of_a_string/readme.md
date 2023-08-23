[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2575\. Find the Divisibility Array of a String

Medium

You are given a **0-indexed** string `word` of length `n` consisting of digits, and a positive integer `m`.

The **divisibility array** `div` of `word` is an integer array of length `n` such that:

*   `div[i] = 1` if the **numeric value** of `word[0,...,i]` is divisible by `m`, or
*   `div[i] = 0` otherwise.

Return _the divisibility array of_ `word`.

**Example 1:**

**Input:** word = "998244353", m = 3

**Output:** [1,1,0,0,0,1,1,0,0]

**Explanation:** There are only 4 prefixes that are divisible by 3: "9", "99", "998244", and "9982443".

**Example 2:**

**Input:** word = "1010", m = 10

**Output:** [0,1,0,1]

**Explanation:** There are only 2 prefixes that are divisible by 10: "10", and "1010".

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `word.length == n`
*   `word` consists of digits from `0` to `9`
*   <code>1 <= m <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int[] divisibilityArray(String word, int m) {
        int n = word.length();
        long modulo = 0;
        int[] result = new int[n];
        for (int i = 0; i < n; i++) {
            int numericValue = word.charAt(i) - '0';
            modulo = (modulo * 10 + numericValue) % m;
            if (modulo == 0) {
                result[i] = 1;
            }
        }
        return result;
    }
}
```