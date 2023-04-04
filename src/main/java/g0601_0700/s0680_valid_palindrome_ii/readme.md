[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 680\. Valid Palindrome II

Easy

Given a string `s`, return `true` _if the_ `s` _can be palindrome after deleting **at most one** character from it_.

**Example 1:**

**Input:** s = "aba"

**Output:** true

**Example 2:**

**Input:** s = "abca"

**Output:** true

**Explanation:** You could delete the character 'c'.

**Example 3:**

**Input:** s = "abc"

**Output:** false

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public boolean validPalindrome(String s) {
        int l = 0;
        int r = s.length() - 1;
        while (l < r) {
            if (s.charAt(l) != s.charAt(r)) {
                return isPalindrome(s.substring(l + 1, r + 1)) || isPalindrome(s.substring(l, r));
            }
            l++;
            r--;
        }
        return true;
    }

    private boolean isPalindrome(String s) {
        int l = 0;
        int r = s.length() - 1;
        while (l < r) {
            if (s.charAt(l) != s.charAt(r)) {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
}
```