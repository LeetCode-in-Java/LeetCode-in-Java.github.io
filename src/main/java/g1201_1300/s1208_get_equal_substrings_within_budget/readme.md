[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1208\. Get Equal Substrings Within Budget

Medium

You are given two strings `s` and `t` of the same length and an integer `maxCost`.

You want to change `s` to `t`. Changing the <code>i<sup>th</sup></code> character of `s` to <code>i<sup>th</sup></code> character of `t` costs `|s[i] - t[i]|` (i.e., the absolute difference between the ASCII values of the characters).

Return _the maximum length of a substring of_ `s` _that can be changed to be the same as the corresponding substring of_ `t` _with a cost less than or equal to_ `maxCost`. If there is no substring from `s` that can be changed to its corresponding substring from `t`, return `0`.

**Example 1:**

**Input:** s = "abcd", t = "bcdf", maxCost = 3

**Output:** 3

**Explanation:** "abc" of s can change to "bcd". That costs 3, so the maximum length is 3.

**Example 2:**

**Input:** s = "abcd", t = "cdef", maxCost = 3

**Output:** 1

**Explanation:** Each character in s costs 2 to change to character in t, so the maximum length is 1.

**Example 3:**

**Input:** s = "abcd", t = "acde", maxCost = 0

**Output:** 1

**Explanation:** You cannot make any change, so the maximum length is 1.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `t.length == s.length`
*   <code>0 <= maxCost <= 10<sup>6</sup></code>
*   `s` and `t` consist of only lowercase English letters.

## Solution

```java
public class Solution {
    public int equalSubstring(String s, String t, int maxCost) {
        int start = 0;
        int end = 0;
        int currCost = 0;
        int maxLength = Integer.MIN_VALUE;
        while (end < s.length()) {
            currCost += Math.abs(s.charAt(end) - t.charAt(end));
            while (currCost > maxCost) {
                currCost -= Math.abs(s.charAt(start) - t.charAt(start));
                start++;
            }
            if (end - start + 1 > maxLength) {
                maxLength = Math.max(end - start + 1, maxLength);
            }
            end++;
        }
        return maxLength;
    }
}
```