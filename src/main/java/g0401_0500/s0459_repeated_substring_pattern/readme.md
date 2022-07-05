[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 459\. Repeated Substring Pattern

Easy

Given a string `s`, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

**Example 1:**

**Input:** s = "abab"

**Output:** true

**Explanation:** It is the substring "ab" twice.

**Example 2:**

**Input:** s = "aba"

**Output:** false

**Example 3:**

**Input:** s = "abcabcabcabc"

**Output:** true

**Explanation:** It is the substring "abc" four times or the substring "abcabc" twice.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int n = s.length();
        if (n < 2) {
            return false;
        }
        int i = 0;
        while (i < (n + 1) / 2) {
            if (n % (i + 1) != 0) {
                i++;
                continue;
            }
            boolean match = true;
            String substring = s.substring(0, i + 1);
            int skippedI = i;
            for (int j = i + 1; j < n; j += i + 1) {
                if (!s.substring(j, j + i + 1).equals(substring)) {
                    match = false;
                    break;
                }
                skippedI += i + 1;
            }
            if (match) {
                return true;
            }
            i = skippedI;
            i++;
        }
        return false;
    }
}
```