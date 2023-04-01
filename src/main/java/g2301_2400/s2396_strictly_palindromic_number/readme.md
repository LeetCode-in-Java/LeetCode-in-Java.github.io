[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2396\. Strictly Palindromic Number

Medium

An integer `n` is **strictly palindromic** if, for **every** base `b` between `2` and `n - 2` (**inclusive**), the string representation of the integer `n` in base `b` is **palindromic**.

Given an integer `n`, return `true` _if_ `n` _is **strictly palindromic** and_ `false` _otherwise_.

A string is **palindromic** if it reads the same forward and backward.

**Example 1:**

**Input:** n = 9

**Output:** false

**Explanation:** In base 2: 9 = 1001 (base 2), which is palindromic.

In base 3: 9 = 100 (base 3), which is not palindromic.

Therefore, 9 is not strictly palindromic so we return false.

Note that in bases 4, 5, 6, and 7, n = 9 is also not palindromic. 

**Example 2:**

**Input:** n = 4

**Output:** false

**Explanation:** We only consider base 2: 4 = 100 (base 2), which is not palindromic.

Therefore, we return false. 

**Constraints:**

*   <code>4 <= n <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public boolean isStrictlyPalindromic(int n) {
        for (int i = 2; i <= n - 2; i++) {
            String num = Integer.toString(i);
            String s = baseConversion(num, 10, i);
            if (!checkPalindrome(s)) {
                return false;
            }
        }
        return true;
    }

    private String baseConversion(String number, int sBase, int dBase) {
        // Parse the number with source radix
        // and return in specified radix(base)
        return Integer.toString(Integer.parseInt(number, sBase), dBase);
    }

    private boolean checkPalindrome(String s) {
        int start = 0;
        int end = s.length() - 1;
        while (start <= end) {
            if (s.charAt(start) != s.charAt(end)) {
                return false;
            }
            start++;
            end--;
        }
        return true;
    }
}
```