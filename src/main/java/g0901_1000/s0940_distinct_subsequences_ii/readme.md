[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 940\. Distinct Subsequences II

Hard

Given a string s, return _the number of **distinct non-empty subsequences** of_ `s`. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not.

**Example 1:**

**Input:** s = "abc"

**Output:** 7

**Explanation:** The 7 distinct subsequences are "a", "b", "c", "ab", "ac", "bc", and "abc".

**Example 2:**

**Input:** s = "aba"

**Output:** 6

**Explanation:** The 6 distinct subsequences are "a", "b", "ab", "aa", "ba", and "aba".

**Example 3:**

**Input:** s = "aaa"

**Output:** 3

**Explanation:** The 3 distinct subsequences are "a", "aa" and "aaa".

**Constraints:**

*   `1 <= s.length <= 2000`
*   `s` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public int distinctSubseqII(String str) {
        int m = (int) (1e9) + 7;
        int prev = 1;
        int[] subseqAtPrevIndexOfMyLastOcc = new int[26];
        for (int i = 1; i <= str.length(); i++) {
            int crntStrIdx = i - 1;
            int subtract = subseqAtPrevIndexOfMyLastOcc[str.charAt(crntStrIdx) - 'a'];
            int curr = (2 * prev) % m;
            curr = (curr + m - subtract) % m;
            subseqAtPrevIndexOfMyLastOcc[str.charAt(crntStrIdx) - 'a'] = prev;
            prev = curr;
        }
        return prev - 1;
    }
}
```