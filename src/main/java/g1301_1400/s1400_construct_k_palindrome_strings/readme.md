[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1400\. Construct K Palindrome Strings

Medium

Given a string `s` and an integer `k`, return `true` _if you can use all the characters in_ `s` _to construct_ `k` _palindrome strings or_ `false` _otherwise_.

**Example 1:**

**Input:** s = "annabelle", k = 2

**Output:** true

**Explanation:** You can construct two palindromes using all characters in s. Some possible constructions "anna" + "elble", "anbna" + "elle", "anellena" + "b"

**Example 2:**

**Input:** s = "leetcode", k = 3

**Output:** false

**Explanation:** It is impossible to construct 3 palindromes using all the characters of s.

**Example 3:**

**Input:** s = "true", k = 4

**Output:** true

**Explanation:** The only possible solution is to put each character in a separate string.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters.
*   <code>1 <= k <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public boolean canConstruct(String s, int k) {
        if (s.length() == k) {
            // if size is same as k we can separate out all letters
            return true;
        }
        if (s.length() < k) {
            // if size is less than it is not possible
            return false;
        }
        // count occurrence of each letter
        int[] count = new int[26];
        for (char curr : s.toCharArray()) {
            count[curr - 'a']++;
        }
        // reduce k whenever count is odd
        for (int i = 0; i < 26; i++) {
            if (count[i] % 2 != 0) {
                k--;
            }
        }
        // we can have max k odd characters
        return k >= 0;
    }
}
```