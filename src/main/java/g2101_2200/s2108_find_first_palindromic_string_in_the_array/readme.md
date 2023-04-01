[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2108\. Find First Palindromic String in the Array

Easy

Given an array of strings `words`, return _the first **palindromic** string in the array_. If there is no such string, return _an **empty string**_ `""`.

A string is **palindromic** if it reads the same forward and backward.

**Example 1:**

**Input:** words = ["abc","car","ada","racecar","cool"]

**Output:** "ada"

**Explanation:** The first string that is palindromic is "ada". Note that "racecar" is also palindromic, but it is not the first.

**Example 2:**

**Input:** words = ["notapalindrome","racecar"]

**Output:** "racecar"

**Explanation:** The first and only string that is palindromic is "racecar".

**Example 3:**

**Input:** words = ["def","ghi"]

**Output:** ""

**Explanation:** There are no palindromic strings, so the empty string is returned.

**Constraints:**

*   `1 <= words.length <= 100`
*   `1 <= words[i].length <= 100`
*   `words[i]` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public static boolean isPalindrome(String s) {
        int len = s.length();
        for (int i = 0, j = len - 1; i <= len / 2 && j >= len / 2; i++, j--) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
        }
        return true;
    }

    public String firstPalindrome(String[] words) {
        for (String word : words) {
            if (isPalindrome(word)) {
                return word;
            }
        }
        return "";
    }
}
```