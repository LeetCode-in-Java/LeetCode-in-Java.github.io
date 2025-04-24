[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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
        char[] c = s.toCharArray();
        int n = c.length;
        int[] a = new int[n];
        int max = 0;
        int i = 1;
        while (i < n) {
            if (c[max] == c[i]) {
                max++;
                a[i] = max;
                i++;
            } else {
                if (max > 0) {
                    max = a[max - 1];
                } else {
                    a[i] = 0;
                    i++;
                }
            }
        }
        return s.substring(0, a[n - 1]);
    }
}
```