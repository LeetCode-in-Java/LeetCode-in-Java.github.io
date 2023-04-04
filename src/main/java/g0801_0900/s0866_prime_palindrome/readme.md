[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 866\. Prime Palindrome

Medium

Given an integer n, return _the smallest **prime palindrome** greater than or equal to_ `n`.

An integer is **prime** if it has exactly two divisors: `1` and itself. Note that `1` is not a prime number.

*   For example, `2`, `3`, `5`, `7`, `11`, and `13` are all primes.

An integer is a **palindrome** if it reads the same from left to right as it does from right to left.

*   For example, `101` and `12321` are palindromes.

The test cases are generated so that the answer always exists and is in the range <code>[2, 2 * 10<sup>8</sup>]</code>.

**Example 1:**

**Input:** n = 6

**Output:** 7

**Example 2:**

**Input:** n = 8

**Output:** 11

**Example 3:**

**Input:** n = 13

**Output:** 101

**Constraints:**

*   <code>1 <= n <= 10<sup>8</sup></code>

## Solution

```java
public class Solution {
    private boolean isPrime(int n) {
        if (n % 2 == 0) {
            return n == 2;
        }
        for (int i = 3, s = (int) Math.sqrt(n); i <= s; i += 2) {
            if (n % i == 0) {
                return false;
            }
        }
        return true;
    }

    private int next(int num) {
        char[] s = String.valueOf(num + 1).toCharArray();
        for (int i = 0, n = s.length; i < (n >> 1); i++) {
            while (s[i] != s[n - 1 - i]) {
                increment(s, n - 1 - i);
            }
        }
        return Integer.parseInt(new String(s));
    }

    private void increment(char[] s, int i) {
        while (s[i] == '9') {
            s[i--] = '0';
        }
        s[i]++;
    }

    public int primePalindrome(int n) {
        if (n <= 2) {
            return 2;
        }
        int p = next(n - 1);
        while (!isPrime(p)) {
            if (10_000_000 <= p && p < 100_000_000) {
                p = 100_000_000;
            }
            p = next(p);
        }
        return p;
    }
}
```