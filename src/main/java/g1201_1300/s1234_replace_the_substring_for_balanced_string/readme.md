[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1234\. Replace the Substring for Balanced String

Medium

You are given a string s of length `n` containing only four kinds of characters: `'Q'`, `'W'`, `'E'`, and `'R'`.

A string is said to be **balanced** if each of its characters appears `n / 4` times where `n` is the length of the string.

Return _the minimum length of the substring that can be replaced with **any** other string of the same length to make_ `s` _**balanced**_. If s is already **balanced**, return `0`.

**Example 1:**

**Input:** s = "QWER"

**Output:** 0

**Explanation:** s is already balanced.

**Example 2:**

**Input:** s = "QQWE"

**Output:** 1

**Explanation:** We need to replace a 'Q' to 'R', so that "RQWE" (or "QRWE") is balanced.

**Example 3:**

**Input:** s = "QQQW"

**Output:** 2

**Explanation:** We can replace the first "QQ" to "ER".

**Constraints:**

*   `n == s.length`
*   <code>4 <= n <= 10<sup>5</sup></code>
*   `n` is a multiple of `4`.
*   `s` contains only `'Q'`, `'W'`, `'E'`, and `'R'`.

## Solution

```java
public class Solution {
    public int balancedString(String s) {
        int n = s.length();
        int ans = n;
        int excess = 0;
        int[] cnt = new int[128];
        cnt['Q'] = cnt['W'] = cnt['E'] = cnt['R'] = -n / 4;
        for (char ch : s.toCharArray()) {
            if (++cnt[ch] == 1) {
                excess++;
            }
        }
        if (excess == 0) {
            return 0;
        }
        int i = 0;
        int j = 0;
        while (i < n) {
            if (--cnt[s.charAt(i)] == 0) {
                excess--;
            }
            while (excess == 0) {
                if (++cnt[s.charAt(j)] == 1) {
                    excess++;
                }
                ans = Math.min(i - j + 1, ans);
                j++;
            }
            i++;
        }

        return ans;
    }
}
```