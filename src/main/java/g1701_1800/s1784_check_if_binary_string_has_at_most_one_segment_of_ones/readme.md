[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1784\. Check if Binary String Has at Most One Segment of Ones

Easy

Given a binary string `s` **without leading zeros**, return `true` _if_ `s` _contains **at most one contiguous segment of ones**_. Otherwise, return `false`.

**Example 1:**

**Input:** s = "1001"

**Output:** false

**Explanation:** The ones do not form a contiguous segment.

**Example 2:**

**Input:** s = "110"

**Output:** true

**Constraints:**

*   `1 <= s.length <= 100`
*   `s[i]` is either `'0'` or `'1'`.
*   `s[0]` is `'1'`.

## Solution

```java
public class Solution {
    public boolean checkOnesSegment(String s) {
        boolean metOne = false;
        int i = 0;
        while (i < s.length()) {
            if (s.charAt(i) == '1' && metOne) {
                return false;
            }
            if (s.charAt(i) == '1') {
                metOne = true;
                while (i < s.length() && s.charAt(i) == '1') {
                    i++;
                }
            }
            i++;
        }
        return true;
    }
}
```