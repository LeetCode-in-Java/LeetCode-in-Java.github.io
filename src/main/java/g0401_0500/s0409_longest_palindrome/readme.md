[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 409\. Longest Palindrome

Easy

Given a string `s` which consists of lowercase or uppercase letters, return _the length of the **longest palindrome**_ that can be built with those letters.

Letters are **case sensitive**, for example, `"Aa"` is not considered a palindrome here.

**Example 1:**

**Input:** s = "abccccdd"

**Output:** 7

**Explanation:** One longest palindrome that can be built is "dccaccd", whose length is 7. 

**Example 2:**

**Input:** s = "a"

**Output:** 1 

**Example 3:**

**Input:** s = "bb"

**Output:** 2 

**Constraints:**

*   `1 <= s.length <= 2000`
*   `s` consists of lowercase **and/or** uppercase English letters only.

## Solution

```java
import java.util.BitSet;

public class Solution {
    public int longestPalindrome(String s) {
        BitSet set = new BitSet(60);
        for (char c : s.toCharArray()) {
            set.flip(c - 'A');
        }
        if (set.isEmpty()) {
            return s.length();
        }
        return s.length() - set.cardinality() + 1;
    }
}
```