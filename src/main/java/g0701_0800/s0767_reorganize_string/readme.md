[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 767\. Reorganize String

Medium

Given a string `s`, rearrange the characters of `s` so that any two adjacent characters are not the same.

Return _any possible rearrangement of_ `s` _or return_ `""` _if not possible_.

**Example 1:**

**Input:** s = "aab"

**Output:** "aba"

**Example 2:**

**Input:** s = "aaab"

**Output:** ""

**Constraints:**

*   `1 <= s.length <= 500`
*   `s` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public String reorganizeString(String s) {
        int[] hash = new int[26];
        for (int i = 0; i < s.length(); i++) {
            hash[s.charAt(i) - 'a']++;
        }
        int max = 0;
        int letter = 0;
        for (int i = 0; i < hash.length; i++) {
            if (hash[i] > max) {
                max = hash[i];
                letter = i;
            }
        }
        if (max > (s.length() + 1) / 2) {
            return "";
        }
        char[] res = new char[s.length()];
        int idx = 0;
        while (hash[letter] > 0) {
            res[idx] = (char) (letter + 'a');
            idx += 2;
            hash[letter]--;
        }
        for (int i = 0; i < hash.length; i++) {
            while (hash[i] > 0) {
                if (idx >= res.length) {
                    idx = 1;
                }
                res[idx] = (char) (i + 'a');
                idx += 2;
                hash[i]--;
            }
        }
        return String.valueOf(res);
    }
}
```