[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3504\. Longest Palindrome After Substring Concatenation II

Hard

You are given two strings, `s` and `t`.

You can create a new string by selecting a **substring** from `s` (possibly empty) and a substring from `t` (possibly empty), then concatenating them **in order**.

Return the length of the **longest** palindrome that can be formed this way.

**Example 1:**

**Input:** s = "a", t = "a"

**Output:** 2

**Explanation:**

Concatenating `"a"` from `s` and `"a"` from `t` results in `"aa"`, which is a palindrome of length 2.

**Example 2:**

**Input:** s = "abc", t = "def"

**Output:** 1

**Explanation:**

Since all characters are different, the longest palindrome is any single character, so the answer is 1.

**Example 3:**

**Input:** s = "b", t = "aaaa"

**Output:** 4

**Explanation:**

Selecting "`aaaa`" from `t` is the longest palindrome, so the answer is 4.

**Example 4:**

**Input:** s = "abcde", t = "ecdba"

**Output:** 5

**Explanation:**

Concatenating `"abc"` from `s` and `"ba"` from `t` results in `"abcba"`, which is a palindrome of length 5.

**Constraints:**

*   `1 <= s.length, t.length <= 1000`
*   `s` and `t` consist of lowercase English letters.

## Solution

```java
public class Solution {
    private int[] sPa;
    private int[] tPa;
    private char[] ss;
    private char[] tt;

    public int longestPalindrome(String s, String t) {
        final int sLen = s.length();
        final int tLen = t.length();
        ss = s.toCharArray();
        tt = t.toCharArray();
        int[][] palindrome = new int[sLen][tLen + 1];
        sPa = new int[sLen];
        tPa = new int[tLen];
        int maxLen = 1;
        for (int j = 0; j < tLen; j++) {
            if (ss[0] == tt[j]) {
                palindrome[0][j] = 2;
                sPa[0] = 2;
                tPa[j] = 2;
                maxLen = 2;
            }
        }
        for (int i = 1; i < sLen; i++) {
            for (int j = 0; j < tLen; j++) {
                if (ss[i] == tt[j]) {
                    palindrome[i][j] = 2 + palindrome[i - 1][j + 1];
                    sPa[i] = Math.max(sPa[i], palindrome[i][j]);
                    tPa[j] = Math.max(tPa[j], palindrome[i][j]);
                    maxLen = Math.max(maxLen, palindrome[i][j]);
                }
            }
        }
        for (int i = 0; i < sLen - 1; i++) {
            int len = maxS(i, i + 1);
            maxLen = Math.max(maxLen, len);
        }
        for (int i = 1; i < sLen; i++) {
            int len = maxS(i - 1, i + 1) + 1;
            maxLen = Math.max(maxLen, len);
        }
        for (int j = 0; j < tLen - 1; j++) {
            int len = maxT(j, j + 1);
            maxLen = Math.max(maxLen, len);
        }
        for (int j = 0; j < tLen - 1; j++) {
            int len = maxT(j - 1, j + 1) + 1;
            maxLen = Math.max(maxLen, len);
        }
        return maxLen;
    }

    private int maxS(int left, int right) {
        int len = 0;
        while (left >= 0 && right < ss.length && ss[left] == ss[right]) {
            len += 2;
            left--;
            right++;
        }
        if (left >= 0) {
            len += sPa[left];
        }
        return len;
    }

    private int maxT(int left, int right) {
        int len = 0;
        while (left >= 0 && right < tt.length && tt[left] == tt[right]) {
            len += 2;
            left--;
            right++;
        }
        if (right < tt.length) {
            len += tPa[right];
        }
        return len;
    }
}
```