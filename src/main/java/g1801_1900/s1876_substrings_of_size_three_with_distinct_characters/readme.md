[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1876\. Substrings of Size Three with Distinct Characters

Easy

A string is **good** if there are no repeated characters.

Given a string `s`, return _the number of **good substrings** of length **three** in_ `s`.

Note that if there are multiple occurrences of the same substring, every occurrence should be counted.

A **substring** is a contiguous sequence of characters in a string.

**Example 1:**

**Input:** s = "xyzzaz"

**Output:** 1

**Explanation:** There are 4 substrings of size 3: "xyz", "yzz", "zza", and "zaz". The only good substring of length 3 is "xyz".

**Example 2:**

**Input:** s = "aababcabc"

**Output:** 4

**Explanation:** There are 7 substrings of size 3: "aab", "aba", "bab", "abc", "bca", "cab", and "abc". The good substrings are "abc", "bca", "cab", and "abc".

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public int countGoodSubstrings(String s) {
        int count = 0;
        for (int i = 0; i < s.length() - 2; i++) {
            String candidate = s.substring(i, i + 3);
            if (candidate.charAt(0) != candidate.charAt(1)
                    && candidate.charAt(0) != candidate.charAt(2)
                    && candidate.charAt(1) != candidate.charAt(2)) {
                count++;
            }
        }
        return count;
    }
}
```