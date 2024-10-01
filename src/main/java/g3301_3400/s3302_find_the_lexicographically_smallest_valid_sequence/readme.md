[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3302\. Find the Lexicographically Smallest Valid Sequence

Medium

You are given two strings `word1` and `word2`.

A string `x` is called **almost equal** to `y` if you can change **at most** one character in `x` to make it _identical_ to `y`.

A sequence of indices `seq` is called **valid** if:

*   The indices are sorted in **ascending** order.
*   _Concatenating_ the characters at these indices in `word1` in **the same** order results in a string that is **almost equal** to `word2`.

Return an array of size `word2.length` representing the lexicographically smallest **valid** sequence of indices. If no such sequence of indices exists, return an **empty** array.

**Note** that the answer must represent the _lexicographically smallest array_, **not** the corresponding string formed by those indices.

**Example 1:**

**Input:** word1 = "vbcca", word2 = "abc"

**Output:** [0,1,2]

**Explanation:**

The lexicographically smallest valid sequence of indices is `[0, 1, 2]`:

*   Change `word1[0]` to `'a'`.
*   `word1[1]` is already `'b'`.
*   `word1[2]` is already `'c'`.

**Example 2:**

**Input:** word1 = "bacdc", word2 = "abc"

**Output:** [1,2,4]

**Explanation:**

The lexicographically smallest valid sequence of indices is `[1, 2, 4]`:

*   `word1[1]` is already `'a'`.
*   Change `word1[2]` to `'b'`.
*   `word1[4]` is already `'c'`.

**Example 3:**

**Input:** word1 = "aaaaaa", word2 = "aaabc"

**Output:** []

**Explanation:**

There is no valid sequence of indices.

**Example 4:**

**Input:** word1 = "abc", word2 = "ab"

**Output:** [0,1]

**Constraints:**

*   <code>1 <= word2.length < word1.length <= 3 * 10<sup>5</sup></code>
*   `word1` and `word2` consist only of lowercase English letters.

## Solution

```java
public class Solution {
    public int[] validSequence(String word1, String word2) {
        char[] c1 = word1.toCharArray();
        char[] c2 = word2.toCharArray();
        int[] dp = new int[c1.length + 1];
        int j = c2.length - 1;
        for (int i = c1.length - 1; i >= 0; i--) {
            if (j >= 0 && c1[i] == c2[j]) {
                dp[i] = dp[i + 1] + 1;
                j--;
            } else {
                dp[i] = dp[i + 1];
            }
        }
        int[] ans = new int[c2.length];
        int i = 0;
        j = 0;
        while (i < c1.length && j < c2.length) {
            if (c1[i] == c2[j]) {
                ans[j] = i;
                j++;
            } else {
                if (dp[i + 1] >= c2.length - 1 - j) {
                    ans[j] = i;
                    j++;
                    i++;
                    break;
                }
            }
            i++;
        }
        if (j < c2.length && i == c1.length) {
            return new int[0];
        }
        while (j < c2.length && i < c1.length) {
            if (c2[j] == c1[i]) {
                ans[j] = i;
                j++;
            }
            i++;
        }
        return ans;
    }
}
```