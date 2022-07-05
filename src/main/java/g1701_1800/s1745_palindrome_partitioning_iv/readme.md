[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1745\. Palindrome Partitioning IV

Hard

Given a string `s`, return `true` _if it is possible to split the string_ `s` _into three **non-empty** palindromic substrings. Otherwise, return_ `false`.

A string is said to be palindrome if it the same string when reversed.

**Example 1:**

**Input:** s = "abcbdd"

**Output:** true

**Explanation:** "abcbdd" = "a" + "bcb" + "dd", and all three substrings are palindromes.

**Example 2:**

**Input:** s = "bcbddxy"

**Output:** false

**Explanation:** s cannot be split into 3 palindromes.

**Constraints:**

*   `3 <= s.length <= 2000`
*   `s` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public boolean checkPartitioning(String s) {
        final int len = s.length();
        char[] ch = s.toCharArray();
        int[] dp = new int[len + 1];
        dp[0] = 0x01;
        for (int i = 0; i < len; i++) {
            for (int l : new int[] {i - 1, i}) {
                int r = i;
                while (l >= 0 && r < len && ch[l] == ch[r]) {
                    dp[r + 1] |= dp[l] << 1;
                    l--;
                    r++;
                }
            }
        }
        return (dp[len] & 0x08) == 0x08;
    }
}
```