[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2486\. Append Characters to String to Make Subsequence

Medium

You are given two strings `s` and `t` consisting of only lowercase English letters.

Return _the minimum number of characters that need to be appended to the end of_ `s` _so that_ `t` _becomes a **subsequence** of_ `s`.

A **subsequence** is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

**Example 1:**

**Input:** s = "coaching", t = "coding"

**Output:** 4

**Explanation:** Append the characters "ding" to the end of s so that s = "coachingding". Now, t is a subsequence of s ("<ins>**co**</ins>aching<ins>**ding**</ins>"). It can be shown that appending any 3 characters to the end of s will never make t a subsequence.

**Example 2:**

**Input:** s = "abcde", t = "a"

**Output:** 0

**Explanation:** t is already a subsequence of s ("<ins>**a**</ins>bcde").

**Example 3:**

**Input:** s = "z", t = "abcde"

**Output:** 5

**Explanation:** Append the characters "abcde" to the end of s so that s = "zabcde". Now, t is a subsequence of s ("z<ins>**abcde**</ins>"). It can be shown that appending any 4 characters to the end of s will never make t a subsequence.

**Constraints:**

*   <code>1 <= s.length, t.length <= 10<sup>5</sup></code>
*   `s` and `t` consist only of lowercase English letters.

## Solution

```java
public class Solution {
    public int appendCharacters(String s, String t) {
        int lengthOfT = t.length();
        int indexOfT = 0;
        int indexOfS = 0;
        int position;
        if (s.contains(t)) {
            return 0;
        }
        while (indexOfT < lengthOfT) {
            position = s.indexOf(t.charAt(indexOfT), indexOfS);
            if (position < 0) {
                return lengthOfT - indexOfT;
            }
            indexOfS = position;
            indexOfT++;
            indexOfS++;
        }
        return lengthOfT - (indexOfT);
    }
}
```