[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 479\. Largest Palindrome Product

Hard

Given an integer n, return _the **largest palindromic integer** that can be represented as the product of two `n`\-digits integers_. Since the answer can be very large, return it **modulo** `1337`.

**Example 1:**

**Input:** n = 2

**Output:** 987 Explanation: 99 x 91 = 9009, 9009 % 1337 = 987

**Example 2:**

**Input:** n = 1

**Output:** 9

**Constraints:**

*   `1 <= n <= 8`

## Solution

```java
public class Solution {
    public int largestPalindrome(int n) {
        long pow10 = (long) Math.pow(10, n);
        long max = (pow10 - 1) * (pow10 - (long) Math.sqrt(pow10) + 1);
        long left = (max / pow10);
        long t = pow10 / 11;
        t -= ~t & 1;
        for (long i = left; i > 0; i--) {
            for (long j = t, num = gen(i); j >= i / 11; j -= 2) {
                if (num % j == 0) {
                    return (int) (num % 1337);
                }
            }
        }
        return 9;
    }

    private long gen(long x) {
        long r = x;
        while (x > 0) {
            r = r * 10 + x % 10;
            x /= 10;
        }
        return r;
    }
}
```