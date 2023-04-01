[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1163\. Last Substring in Lexicographical Order

Hard

Given a string `s`, return _the last substring of_ `s` _in lexicographical order_.

**Example 1:**

**Input:** s = "abab"

**Output:** "bab"

**Explanation:** The substrings are ["a", "ab", "aba", "abab", "b", "ba", "bab"]. The lexicographically maximum substring is "bab".

**Example 2:**

**Input:** s = "leetcode"

**Output:** "tcode"

**Constraints:**

*   <code>1 <= s.length <= 4 * 10<sup>5</sup></code>
*   `s` contains only lowercase English letters.

## Solution

```java
public class Solution {
    public String lastSubstring(String str) {
        char[] s = str.toCharArray();
        int i = 0;
        int j = i + 1;
        int l = 0;
        int n = s.length;
        while (j + l < n) {
            if (s[i + l] == s[j + l]) {
                l++;
            } else {
                if (s[i + l] <= s[j + l]) {
                    if (s[j + l] > s[i]) {
                        i = j + l;
                    } else {
                        int p = j - i;
                        i = j + (l / p) * p;
                    }
                }
                j = j + l + 1;
                l = 0;
            }
        }
        return str.substring(i);
    }
}
```