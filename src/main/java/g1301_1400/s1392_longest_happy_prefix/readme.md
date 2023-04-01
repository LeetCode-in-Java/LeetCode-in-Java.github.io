[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1392\. Longest Happy Prefix

Hard

A string is called a **happy prefix** if is a **non-empty** prefix which is also a suffix (excluding itself).

Given a string `s`, return _the **longest happy prefix** of_ `s`. Return an empty string `""` if no such prefix exists.

**Example 1:**

**Input:** s = "level"

**Output:** "l"

**Explanation:** s contains 4 prefix excluding itself ("l", "le", "lev", "leve"), and suffix ("l", "el", "vel", "evel"). The largest prefix which is also suffix is given by "l".

**Example 2:**

**Input:** s = "ababab"

**Output:** "abab"

**Explanation:** "abab" is the largest prefix which is also suffix. They can overlap in the original string.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` contains only lowercase English letters.

## Solution

```java
public class Solution {
    public String longestPrefix(String s) {
        int times = 2;
        long prefixHash = 0;
        long suffixHash = 0;
        long multiplier = 1;
        long len = 0;
        // use some large prime as a modulo to avoid overflow errors, e.g. 10 ^ 9 + 7.
        long mod = 1000000007;
        for (int i = 0; i < s.length() - 1; i++) {
            prefixHash = (prefixHash * times + s.charAt(i)) % mod;
            suffixHash = (multiplier * s.charAt(s.length() - i - 1) + suffixHash) % mod;
            if (prefixHash == suffixHash) {
                len = (long) i + 1;
            }
            multiplier = multiplier * times % mod;
        }
        return s.substring(0, (int) len);
    }
}
```