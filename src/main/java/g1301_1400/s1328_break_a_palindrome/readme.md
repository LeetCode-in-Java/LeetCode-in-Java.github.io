[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1328\. Break a Palindrome

Medium

Given a palindromic string of lowercase English letters `palindrome`, replace **exactly one** character with any lowercase English letter so that the resulting string is **not** a palindrome and that it is the **lexicographically smallest** one possible.

Return _the resulting string. If there is no way to replace a character to make it not a palindrome, return an **empty string**._

A string `a` is lexicographically smaller than a string `b` (of the same length) if in the first position where `a` and `b` differ, `a` has a character strictly smaller than the corresponding character in `b`. For example, `"abcc"` is lexicographically smaller than `"abcd"` because the first position they differ is at the fourth character, and `'c'` is smaller than `'d'`.

**Example 1:**

**Input:** palindrome = "abccba"

**Output:** "aaccba"

**Explanation:** There are many ways to make "abccba" not a palindrome, such as "zbccba", "aaccba", and "abacba". Of all the ways, "aaccba" is the lexicographically smallest.

**Example 2:**

**Input:** palindrome = "a"

**Output:** ""

**Explanation:** There is no way to replace a single character to make "a" not a palindrome, so return an empty string.

**Constraints:**

*   `1 <= palindrome.length <= 1000`
*   `palindrome` consists of only lowercase English letters.

## Solution

```java
public class Solution {
    public String breakPalindrome(String palindrome) {
        if (palindrome.length() <= 1) {
            return "";
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < palindrome.length(); i++) {
            char ch = palindrome.charAt(i);
            if (ch != 'a' && i != palindrome.length() - 1 - i) {
                sb.append('a');
                sb.append(palindrome.substring(i + 1));
                return sb.toString();
            } else {
                sb.append(ch);
            }
        }
        sb.deleteCharAt(palindrome.length() - 1);
        sb.append('b');
        return sb.toString();
    }
}
```