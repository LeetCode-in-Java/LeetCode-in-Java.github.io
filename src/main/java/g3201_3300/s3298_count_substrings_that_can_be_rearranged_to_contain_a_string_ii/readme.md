[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3298\. Count Substrings That Can Be Rearranged to Contain a String II

Hard

You are given two strings `word1` and `word2`.

A string `x` is called **valid** if `x` can be rearranged to have `word2` as a prefix.

Return the total number of **valid** substrings of `word1`.

**Note** that the memory limits in this problem are **smaller** than usual, so you **must** implement a solution with a _linear_ runtime complexity.

**Example 1:**

**Input:** word1 = "bcca", word2 = "abc"

**Output:** 1

**Explanation:**

The only valid substring is `"bcca"` which can be rearranged to `"abcc"` having `"abc"` as a prefix.

**Example 2:**

**Input:** word1 = "abcabc", word2 = "abc"

**Output:** 10

**Explanation:**

All the substrings except substrings of size 1 and size 2 are valid.

**Example 3:**

**Input:** word1 = "abcabc", word2 = "aaabc"

**Output:** 0

**Constraints:**

*   <code>1 <= word1.length <= 10<sup>6</sup></code>
*   <code>1 <= word2.length <= 10<sup>4</sup></code>
*   `word1` and `word2` consist only of lowercase English letters.

## Solution

```java
public class Solution {
    public long validSubstringCount(String word1, String word2) {
        char[] ar = word1.toCharArray();
        int n = ar.length;
        char[] temp = word2.toCharArray();
        int[] f = new int[26];
        for (char i : temp) {
            f[i - 97]++;
        }
        long ans = 0;
        int needed = temp.length;
        int beg = 0;
        int end = 0;
        while (end < n) {
            if (f[ar[end] - 97]-- > 0) {
                needed--;
            }
            while (needed == 0) {
                // All substrings from [beg, i], where end <= i < n are valid
                ans += n - end;
                // Shrink
                if (f[ar[beg++] - 97]++ == 0) {
                    needed++;
                }
            }
            end++;
        }
        return ans;
    }
}
```