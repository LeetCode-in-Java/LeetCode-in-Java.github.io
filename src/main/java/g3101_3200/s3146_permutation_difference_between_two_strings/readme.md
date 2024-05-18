[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3146\. Permutation Difference between Two Strings

Easy

You are given two strings `s` and `t` such that every character occurs at most once in `s` and `t` is a permutation of `s`.

The **permutation difference** between `s` and `t` is defined as the **sum** of the absolute difference between the index of the occurrence of each character in `s` and the index of the occurrence of the same character in `t`.

Return the **permutation difference** between `s` and `t`.

**Example 1:**

**Input:** s = "abc", t = "bac"

**Output:** 2

**Explanation:**

For `s = "abc"` and `t = "bac"`, the permutation difference of `s` and `t` is equal to the sum of:

*   The absolute difference between the index of the occurrence of `"a"` in `s` and the index of the occurrence of `"a"` in `t`.
*   The absolute difference between the index of the occurrence of `"b"` in `s` and the index of the occurrence of `"b"` in `t`.
*   The absolute difference between the index of the occurrence of `"c"` in `s` and the index of the occurrence of `"c"` in `t`.

That is, the permutation difference between `s` and `t` is equal to `|0 - 1| + |2 - 2| + |1 - 0| = 2`.

**Example 2:**

**Input:** s = "abcde", t = "edbac"

**Output:** 12

**Explanation:** The permutation difference between `s` and `t` is equal to `|0 - 3| + |1 - 2| + |2 - 4| + |3 - 1| + |4 - 0| = 12`.

**Constraints:**

*   `1 <= s.length <= 26`
*   Each character occurs at most once in `s`.
*   `t` is a permutation of `s`.
*   `s` consists only of lowercase English letters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int findPermutationDifference(String s, String t) {
        int[] res = new int[26];
        Arrays.fill(res, -1);
        int sum = 0;
        for (int i = 0; i < s.length(); ++i) {
            res[s.charAt(i) - 'a'] = i;
        }
        for (int i = 0; i < t.length(); ++i) {
            sum += Math.abs(res[t.charAt(i) - 'a'] - i);
        }
        return sum;
    }
}
```