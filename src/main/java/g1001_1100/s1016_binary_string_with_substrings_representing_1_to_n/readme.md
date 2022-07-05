[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1016\. Binary String With Substrings Representing 1 To N

Medium

Given a binary string `s` and a positive integer `n`, return `true` _if the binary representation of all the integers in the range_ `[1, n]` _are **substrings** of_ `s`_, or_ `false` _otherwise_.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "0110", n = 3

**Output:** true

**Example 2:**

**Input:** s = "0110", n = 4

**Output:** false

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s[i]` is either `'0'` or `'1'`.
*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public boolean queryString(String s, int n) {
        for (int i = 1; i <= n; i++) {
            String str = Integer.toBinaryString(i);
            if (!s.contains(str)) {
                return false;
            }
        }
        return true;
    }
}
```