[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2565\. Subsequence With the Minimum Score

Hard

You are given two strings `s` and `t`.

You are allowed to remove any number of characters from the string `t`.

The score of the string is `0` if no characters are removed from the string `t`, otherwise:

*   Let `left` be the minimum index among all removed characters.
*   Let `right` be the maximum index among all removed characters.

Then the score of the string is `right - left + 1`.

Return _the minimum possible score to make_ `t`_a subsequence of_ `s`_._

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of <code>"<ins>a</ins>b<ins>c</ins>d<ins>e</ins>"</code> while `"aec"` is not).

**Example 1:**

**Input:** s = "abacaba", t = "bzaa"

**Output:** 1

**Explanation:** In this example, we remove the character "z" at index 1 (0-indexed). The string t becomes "baa" which is a subsequence of the string "abacaba" and the score is 1 - 1 + 1 = 1. It can be proven that 1 is the minimum score that we can achieve.

**Example 2:**

**Input:** s = "cde", t = "xyz"

**Output:** 3

**Explanation:** In this example, we remove characters "x", "y" and "z" at indices 0, 1, and 2 (0-indexed). The string t becomes "" which is a subsequence of the string "cde" and the score is 2 - 0 + 1 = 3. It can be proven that 3 is the minimum score that we can achieve.

**Constraints:**

*   <code>1 <= s.length, t.length <= 10<sup>5</sup></code>
*   `s` and `t` consist of only lowercase English letters.

## Solution

```java
public class Solution {
    public int minimumScore(String s, String t) {
        int m = s.length();
        int n = t.length();
        int[] left = new int[m];
        int j = 0;
        for (int i = 0; i < m; i++) {
            if (j < n && s.charAt(i) == t.charAt(j)) {
                ++j;
            }
            left[i] = j;
        }
        int[] right = new int[m];
        j = n - 1;
        for (int i = m - 1; i >= 0; i--) {
            if (j >= 0 && s.charAt(i) == t.charAt(j)) {
                --j;
            }
            right[i] = j;
        }
        int min = Math.min(n - left[m - 1], right[0] + 1);
        for (int i = 0; i + 1 < m; i++) {
            min = Math.min(min, Math.max(0, right[i + 1] - left[i] + 1));
        }
        return min;
    }
}
```