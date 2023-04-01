[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 567\. Permutation in String

Medium

Given two strings `s1` and `s2`, return `true` _if_ `s2` _contains a permutation of_ `s1`_, or_ `false` _otherwise_.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

**Example 1:**

**Input:** s1 = "ab", s2 = "eidbaooo"

**Output:** true

**Explanation:** s2 contains one permutation of s1 ("ba").

**Example 2:**

**Input:** s1 = "ab", s2 = "eidboaoo"

**Output:** false

**Constraints:**

*   <code>1 <= s1.length, s2.length <= 10<sup>4</sup></code>
*   `s1` and `s2` consist of lowercase English letters.

## Solution

```java
public class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int n = s1.length();
        int m = s2.length();
        if (n > m) {
            return false;
        }
        int[] cntS1 = new int[26];
        int[] cntS2 = new int[26];
        for (int i = 0; i < n; i++) {
            cntS1[s1.charAt(i) - 'a']++;
        }
        for (int i = 0; i < n; i++) {
            cntS2[s2.charAt(i) - 'a']++;
        }
        if (check(cntS1, cntS2)) {
            return true;
        }
        for (int i = n; i < m; i++) {
            cntS2[s2.charAt(i - n) - 'a']--;
            cntS2[s2.charAt(i) - 'a']++;
            if (check(cntS1, cntS2)) {
                return true;
            }
        }
        return false;
    }

    private boolean check(int[] cnt1, int[] cnt2) {
        for (int i = 0; i < 26; i++) {
            if (cnt1[i] != cnt2[i]) {
                return false;
            }
        }
        return true;
    }
}
```