[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1624\. Largest Substring Between Two Equal Characters

Easy

Given a string `s`, return _the length of the longest substring between two equal characters, excluding the two characters._ If there is no such substring return `-1`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "aa"

**Output:** 0

**Explanation:** The optimal substring here is an empty substring between the two `'a's`.

**Example 2:**

**Input:** s = "abca"

**Output:** 2

**Explanation:** The optimal substring here is "bc".

**Example 3:**

**Input:** s = "cbzxy"

**Output:** -1

**Explanation:** There are no characters that appear twice in s.

**Constraints:**

*   `1 <= s.length <= 300`
*   `s` contains only lowercase English letters.

## Solution

```java
public class Solution {
    public int maxLengthBetweenEqualCharacters(String s) {
        int maxLen = -1;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            int lastIndex = s.lastIndexOf(c);
            if (lastIndex != i) {
                maxLen = Math.max(maxLen, Math.abs(lastIndex - i - 1));
            }
        }
        return maxLen;
    }
}
```