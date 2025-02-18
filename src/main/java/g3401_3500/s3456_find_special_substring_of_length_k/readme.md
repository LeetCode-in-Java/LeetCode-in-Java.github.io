[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3456\. Find Special Substring of Length K

Easy

You are given a string `s` and an integer `k`.

Determine if there exists a **substring** of length **exactly** `k` in `s` that satisfies the following conditions:

1.  The substring consists of **only one distinct character** (e.g., `"aaa"` or `"bbb"`).
2.  If there is a character **immediately before** the substring, it must be different from the character in the substring.
3.  If there is a character **immediately after** the substring, it must also be different from the character in the substring.

Return `true` if such a substring exists. Otherwise, return `false`.

**Example 1:**

**Input:** s = "aaabaaa", k = 3

**Output:** true

**Explanation:**

The substring `s[4..6] == "aaa"` satisfies the conditions.

*   It has a length of 3.
*   All characters are the same.
*   The character before `"aaa"` is `'b'`, which is different from `'a'`.
*   There is no character after `"aaa"`.

**Example 2:**

**Input:** s = "abc", k = 2

**Output:** false

**Explanation:**

There is no substring of length 2 that consists of one distinct character and satisfies the conditions.

**Constraints:**

*   `1 <= k <= s.length <= 100`
*   `s` consists of lowercase English letters only.

## Solution

```java
@SuppressWarnings("java:S1871")
public class Solution {
    public boolean hasSpecialSubstring(String s, int k) {
        int start = 0;
        int end = k;
        while (end <= s.length()) {
            boolean flag = false;
            for (int i = start; i < end - 1; i++) {
                if (s.charAt(i) != s.charAt(i + 1)) {
                    start++;
                    end++;
                    flag = true;
                    break;
                }
            }
            if (flag) {
                continue;
            }
            if (start - 1 >= 0 && s.charAt(start) == s.charAt(start - 1)
                    || end < s.length() && s.charAt(end) == s.charAt(end - 1)) {
                start++;
                end++;
            } else {
                return true;
            }
        }
        return false;
    }
}
```