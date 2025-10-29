[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3722\. Lexicographically Smallest String After Reverse

Medium

You are given a string `s` of length `n` consisting of lowercase English letters.

You must perform **exactly** one operation by choosing any integer `k` such that `1 <= k <= n` and either:

*   reverse the **first** `k` characters of `s`, or
*   reverse the **last** `k` characters of `s`.

Return the **lexicographically smallest** string that can be obtained after **exactly** one such operation.

A string `a` is **lexicographically smaller** than a string `b` if, at the first position where they differ, `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`. If the first `min(a.length, b.length)` characters are the same, then the shorter string is considered lexicographically smaller.

**Example 1:**

**Input:** s = "dcab"

**Output:** "acdb"

**Explanation:**

*   Choose `k = 3`, reverse the first 3 characters.
*   Reverse `"dca"` to `"acd"`, resulting string `s = "acdb"`, which is the lexicographically smallest string achievable.

**Example 2:**

**Input:** s = "abba"

**Output:** "aabb"

**Explanation:**

*   Choose `k = 3`, reverse the last 3 characters.
*   Reverse `"bba"` to `"abb"`, so the resulting string is `"aabb"`, which is the lexicographically smallest string achievable.

**Example 3:**

**Input:** s = "zxy"

**Output:** "xzy"

**Explanation:**

*   Choose `k = 2`, reverse the first 2 characters.
*   Reverse `"zx"` to `"xz"`, so the resulting string is `"xzy"`, which is the lexicographically smallest string achievable.

**Constraints:**

*   `1 <= n == s.length <= 1000`
*   `s` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public String lexSmallest(String s) {
        int n = s.length();
        char[] arr = s.toCharArray();
        char[] best = arr.clone();
        // Check all reverse first k operations
        for (int k = 1; k <= n; k++) {
            if (isBetterReverseFirstK(arr, k, best)) {
                updateBestReverseFirstK(arr, k, best);
            }
        }
        // Check all reverse last k operations
        for (int k = 1; k <= n; k++) {
            if (isBetterReverseLastK(arr, k, best)) {
                updateBestReverseLastK(arr, k, best);
            }
        }
        return new String(best);
    }

    private boolean isBetterReverseFirstK(char[] arr, int k, char[] best) {
        for (int i = 0; i < arr.length; i++) {
            char currentChar = (i < k) ? arr[k - 1 - i] : arr[i];
            if (currentChar < best[i]) {
                return true;
            }
            if (currentChar > best[i]) {
                return false;
            }
        }
        return false;
    }

    private boolean isBetterReverseLastK(char[] arr, int k, char[] best) {
        int n = arr.length;
        for (int i = 0; i < n; i++) {
            char currentChar = (i < n - k) ? arr[i] : arr[n - 1 - (i - (n - k))];
            if (currentChar < best[i]) {
                return true;
            }
            if (currentChar > best[i]) {
                return false;
            }
        }
        return false;
    }

    private void updateBestReverseFirstK(char[] arr, int k, char[] best) {
        for (int i = 0; i < k; i++) {
            best[i] = arr[k - 1 - i];
        }
        if (arr.length - k >= 0) {
            System.arraycopy(arr, k, best, k, arr.length - k);
        }
    }

    private void updateBestReverseLastK(char[] arr, int k, char[] best) {
        int n = arr.length;
        if (n - k >= 0) {
            System.arraycopy(arr, 0, best, 0, n - k);
        }
        for (int i = 0; i < k; i++) {
            best[n - k + i] = arr[n - 1 - i];
        }
    }
}
```