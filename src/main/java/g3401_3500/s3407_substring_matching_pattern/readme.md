[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3407\. Substring Matching Pattern

Easy

You are given a string `s` and a pattern string `p`, where `p` contains **exactly one** `'*'` character.

The `'*'` in `p` can be replaced with any sequence of zero or more characters.

Return `true` if `p` can be made a substring of `s`, and `false` otherwise.

A **substring** is a contiguous **non-empty** sequence of characters within a string.

**Example 1:**

**Input:** s = "leetcode", p = "ee\*e"

**Output:** true

**Explanation:**

By replacing the `'*'` with `"tcod"`, the substring `"eetcode"` matches the pattern.

**Example 2:**

**Input:** s = "car", p = "c\*v"

**Output:** false

**Explanation:**

There is no substring matching the pattern.

**Example 3:**

**Input:** s = "luck", p = "u\*"

**Output:** true

**Explanation:**

The substrings `"u"`, `"uc"`, and `"uck"` match the pattern.

**Constraints:**

*   `1 <= s.length <= 50`
*   `1 <= p.length <= 50`
*   `s` contains only lowercase English letters.
*   `p` contains only lowercase English letters and exactly one `'*'`

## Solution

```java
public class Solution {
    public boolean hasMatch(String s, String p) {
        int index = -1;
        for (int i = 0; i < p.length(); i++) {
            if (p.charAt(i) == '*') {
                index = i;
                break;
            }
        }
        int num1 = fun(s, p.substring(0, index));
        if (num1 == -1) {
            return false;
        }
        int num2 = fun(s.substring(num1), p.substring(index + 1));
        return num2 != -1;
    }

    private int fun(String s, String k) {
        int n = s.length();
        int m = k.length();
        int j = 0;
        for (int i = 0; i <= n - m; i++) {
            for (j = 0; j < m; j++) {
                char ch1 = s.charAt(j + i);
                char ch2 = k.charAt(j);
                if (ch1 != ch2) {
                    break;
                }
            }
            if (j == m) {
                return i + j;
            }
        }
        return -1;
    }
}
```