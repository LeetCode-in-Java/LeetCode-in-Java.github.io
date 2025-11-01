[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 28\. Find the Index of the First Occurrence in a String

Easy

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

**Example 1:**

**Input:** haystack = "sadbutsad", needle = "sad"

**Output:** 0

**Explanation:** "sad" occurs at index 0 and 6. The first occurrence is at index 0, so we return 0. 

**Example 2:**

**Input:** haystack = "leetcode", needle = "leeto"

**Output:** -1

**Explanation:** "leeto" did not occur in "leetcode", so we return -1. 

**Constraints:**

*   <code>1 <= haystack.length, needle.length <= 10<sup>4</sup></code>
*   `haystack` and `needle` consist of only lowercase English characters.

## Solution

```java
public class Solution {
    public int strStr(String haystack, String needle) {
        if (needle.isEmpty()) {
            return 0;
        }
        int m = haystack.length();
        int n = needle.length();
        for (int start = 0; start < m - n + 1; start++) {
            if (haystack.substring(start, start + n).equals(needle)) {
                return start;
            }
        }
        return -1;
    }
}
```