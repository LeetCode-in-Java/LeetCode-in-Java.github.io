[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1910\. Remove All Occurrences of a Substring

Medium

Given two strings `s` and `part`, perform the following operation on `s` until **all** occurrences of the substring `part` are removed:

*   Find the **leftmost** occurrence of the substring `part` and **remove** it from `s`.

Return `s` _after removing all occurrences of_ `part`.

A **substring** is a contiguous sequence of characters in a string.

**Example 1:**

**Input:** s = "daabcbaabcbc", part = "abc"

**Output:** "dab"

**Explanation:** The following operations are done: 

- s = "da**abc**baabcbc", remove "abc" starting at index 2, so s = "dabaabcbc". 

- s = "daba**abc**bc", remove "abc" starting at index 4, so s = "dababc". 

- s = "dab**abc**", remove "abc" starting at index 3, so s = "dab". Now s has no occurrences of "abc".

**Example 2:**

**Input:** s = "axxxxyyyyb", part = "xy"

**Output:** "ab"

**Explanation:** The following operations are done: 

- s = "axxx**xy**yyyb", remove "xy" starting at index 4 so s = "axxxyyyb".

- s = "axx**xy**yyb", remove "xy" starting at index 3 so s = "axxyyb". 

- s = "ax**xy**yb", remove "xy" starting at index 2 so s = "axyb". 

- s = "a**xy**b", remove "xy" starting at index 1 so s = "ab". Now s has no occurrences of "xy".

**Constraints:**

*   `1 <= s.length <= 1000`
*   `1 <= part.length <= 1000`
*   `s` and `part` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public String removeOccurrences(String s, String part) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            sb.append(s.charAt(i));
            if (sb.length() >= part.length()
                    && sb.substring(sb.length() - part.length()).equals(part)) {
                sb.setLength(sb.length() - part.length());
            }
        }
        return sb.toString();
    }
}
```