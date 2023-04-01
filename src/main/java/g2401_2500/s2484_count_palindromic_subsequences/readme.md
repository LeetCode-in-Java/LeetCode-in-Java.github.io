[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2484\. Count Palindromic Subsequences

Hard

Given a string of digits `s`, return _the number of **palindromic subsequences** of_ `s` _having length_ `5`. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Note:**

*   A string is **palindromic** if it reads the same forward and backward.
*   A **subsequence** is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

**Example 1:**

**Input:** s = "103301"

**Output:** 2

**Explanation:** There are 6 possible subsequences of length 5: "10330","10331","10301","10301","13301","03301". Two of them (both equal to "10301") are palindromic.

**Example 2:**

**Input:** s = "0000000"

**Output:** 21

**Explanation:** All 21 subsequences are "00000", which is palindromic.

**Example 3:**

**Input:** s = "9999900000"

**Output:** 2

**Explanation:** The only two palindromic subsequences are "99999" and "00000".

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of digits.

## Solution

```java
public class Solution {
    public int countPalindromes(String s) {
        int n = s.length();
        long ans = 0;
        int mod = (int) 1e9 + 7;
        int[] count = new int[10];
        for (int i = 0; i < n; i++) {
            long m = 0;
            for (int j = n - 1; j > i; j--) {
                if (s.charAt(i) == s.charAt(j)) {
                    ans += (m * (j - i - 1));
                    ans = ans % mod;
                }
                m += count[s.charAt(j) - '0'];
            }
            count[s.charAt(i) - '0']++;
        }
        return (int) ans;
    }
}
```