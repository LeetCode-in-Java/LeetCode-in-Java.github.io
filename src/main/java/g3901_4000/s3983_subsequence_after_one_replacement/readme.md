[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3983\. Subsequence After One Replacement

Medium

You are given two strings `s` and `t` consisting of lowercase English letters.

You may choose **at most** one index in `s` and replace the character at that index with any lowercase English letter.

Return `true` if it is possible to make `s` a subsequence of `t`; otherwise, return `false`.

**Example 1:**

**Input:** s = "cat", t = "chat"

**Output:** true

**Explanation:**

*   Replace `s[1]` from `'a'` to `'h'`. The resulting string is `"cht"`.
*   `"cht"` is a subsequence of `"chat"` because we can match `'c'`, `'h'`, and `'t'` in order.

**Example 2:**

**Input:** s = "plane", t = "apple"

**Output:** false

**Explanation:**

*   The characters `'p'`, `'l'`, and `'e'` can be matched in `t`, but the remaining characters cannot be matched while preserving the required order.
*   Even after replacing any one character in `s`, it is impossible to make `s` a subsequence of `t`.

**Constraints:**

*   <code>1 <= s.length, t.length <= 10<sup>5</sup></code>
*   `s` and `t` consist only of lowercase English letters.

## Solution

```java
public class Solution {
    public boolean canMakeSubsequence(String t, String s) {
        char[] arrS = s.toCharArray();
        char[] arrT = t.toCharArray();
        if (arrS.length < arrT.length) {
            return false;
        }
        int idx1 = 0;
        int idx2 = 0;
        int n = arrT.length;
        for (char curr : arrS) {
            int nextIdx1 = idx1;
            int nextIdx2 = idx2;
            if (idx2 < n && arrT[idx2] == curr) {
                nextIdx2 = Math.max(nextIdx2, idx2 + 1);
            }
            if (idx1 < n) {
                nextIdx2 = Math.max(nextIdx2, idx1 + 1);
            }
            if (idx1 < n && arrT[idx1] == curr) {
                nextIdx1 = idx1 + 1;
            }
            nextIdx2 = Math.max(nextIdx2, nextIdx1);
            idx1 = nextIdx1;
            idx2 = nextIdx2;
        }
        return idx1 == n || idx2 == n;
    }
}
```