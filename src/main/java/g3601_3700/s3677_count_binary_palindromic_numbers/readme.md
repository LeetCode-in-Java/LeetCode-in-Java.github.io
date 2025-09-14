[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3677\. Count Binary Palindromic Numbers

Hard

You are given a **non-negative** integer `n`.

A **non-negative** integer is called **binary-palindromic** if its binary representation (written without leading zeros) reads the same forward and backward.

Return the number of integers `k` such that `0 <= k <= n` and the binary representation of `k` is a palindrome.

**Note:** The number 0 is considered binary-palindromic, and its representation is `"0"`.

**Example 1:**

**Input:** n = 9

**Output:** 6

**Explanation:**

The integers `k` in the range `[0, 9]` whose binary representations are palindromes are:

*   `0 → "0"`
*   `1 → "1"`
*   `3 → "11"`
*   `5 → "101"`
*   `7 → "111"`
*   `9 → "1001"`

All other values in `[0, 9]` have non-palindromic binary forms. Therefore, the count is 6.

**Example 2:**

**Input:** n = 0

**Output:** 1

**Explanation:**

Since `"0"` is a palindrome, the count is 1.

**Constraints:**

*   <code>0 <= n <= 10<sup>15</sup></code>

## Solution

```java
public class Solution {
    private long makePalin(long left, boolean odd) {
        long ans = left;
        if (odd) {
            left = left >> 1;
        }
        while (left > 0) {
            ans = (ans << 1) | (left & 1);
            left = left >> 1;
        }
        return ans;
    }

    public int countBinaryPalindromes(long n) {
        if (n == 0) {
            return 1;
        }
        int len = 64 - Long.numberOfLeadingZeros(n);
        long count = 1;
        for (int i = 1; i < len; i++) {
            int half = (i + 1) / 2;
            count += 1L << (half - 1);
        }
        int half = (len + 1) / 2;
        long prefix = n >> (len - half);
        long palin = makePalin(prefix, len % 2 == 1);
        count += (prefix - (1L << (half - 1)));
        if (palin <= n) {
            ++count;
        }
        return (int) count;
    }
}
```