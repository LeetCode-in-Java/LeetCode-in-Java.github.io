[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 854\. K-Similar Strings

Hard

Strings `s1` and `s2` are `k`**\-similar** (for some non-negative integer `k`) if we can swap the positions of two letters in `s1` exactly `k` times so that the resulting string equals `s2`.

Given two anagrams `s1` and `s2`, return the smallest `k` for which `s1` and `s2` are `k`**\-similar**.

**Example 1:**

**Input:** s1 = "ab", s2 = "ba"

**Output:** 1

**Example 2:**

**Input:** s1 = "abc", s2 = "bca"

**Output:** 2

**Constraints:**

*   `1 <= s1.length <= 20`
*   `s2.length == s1.length`
*   `s1` and `s2` contain only lowercase letters from the set `{'a', 'b', 'c', 'd', 'e', 'f'}`.
*   `s2` is an anagram of `s1`.

## Solution

```java
public class Solution {
    public int kSimilarity(String a, String b) {
        int ans = 0;
        char[] achars = a.toCharArray();
        char[] bchars = b.toCharArray();
        ans += getAllPerfectMatches(achars, bchars);
        for (int i = 0; i < achars.length; i++) {
            if (achars[i] == bchars[i]) {
                continue;
            }
            return ans + checkAllOptions(achars, bchars, i, b);
        }
        return ans;
    }

    private int checkAllOptions(char[] achars, char[] bchars, int i, String b) {
        int ans = Integer.MAX_VALUE;
        for (int j = i + 1; j < achars.length; j++) {
            if (achars[j] == bchars[i] && achars[j] != bchars[j]) {
                swap(achars, i, j);
                ans = Math.min(ans, 1 + kSimilarity(new String(achars), b));
                swap(achars, i, j);
            }
        }
        return ans;
    }

    private int getAllPerfectMatches(char[] achars, char[] bchars) {
        int ans = 0;
        for (int i = 0; i < achars.length; i++) {
            if (achars[i] == bchars[i]) {
                continue;
            }
            for (int j = i + 1; j < achars.length; j++) {
                if (achars[j] == bchars[i] && bchars[j] == achars[i]) {
                    swap(achars, i, j);
                    ans++;
                    break;
                }
            }
        }
        return ans;
    }

    private void swap(char[] a, int i, int j) {
        char temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
}
```