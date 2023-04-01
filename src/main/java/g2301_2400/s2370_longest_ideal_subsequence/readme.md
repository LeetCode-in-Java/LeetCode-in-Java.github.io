[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2370\. Longest Ideal Subsequence

Medium

You are given a string `s` consisting of lowercase letters and an integer `k`. We call a string `t` **ideal** if the following conditions are satisfied:

*   `t` is a **subsequence** of the string `s`.
*   The absolute difference in the alphabet order of every two **adjacent** letters in `t` is less than or equal to `k`.

Return _the length of the **longest** ideal string_.

A **subsequence** is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

**Note** that the alphabet order is not cyclic. For example, the absolute difference in the alphabet order of `'a'` and `'z'` is `25`, not `1`.

**Example 1:**

**Input:** s = "acfgbd", k = 2

**Output:** 4

**Explanation:** The longest ideal string is "acbd". The length of this string is 4, so 4 is returned. Note that "acfgbd" is not ideal because 'c' and 'f' have a difference of 3 in alphabet order.

**Example 2:**

**Input:** s = "abcd", k = 3

**Output:** 4

**Explanation:** The longest ideal string is "abcd". The length of this string is 4, so 4 is returned.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `0 <= k <= 25`
*   `s` consists of lowercase English letters.

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    public int longestIdealString(String s, int k) {
        int ans = 1;
        int[] array = new int[26];
        for (int i = 0; i < s.length(); i++) {
            int curr = s.charAt(i) - 'a';
            int currans = 1;
            int temp = k;
            array[curr] += 1;
            int j = curr - 1;
            while (temp > 0) {
                if (j == -1) {
                    break;
                }
                currans = Math.max(currans, array[j] + 1);
                temp--;
                if (j == 0) {
                    break;
                }
                j--;
            }
            temp = k;
            j = curr + 1;
            while (temp > 0) {
                if (j == 26) {
                    break;
                }
                currans = Math.max(currans, array[j] + 1);
                temp--;
                if (j == 25) {
                    break;
                }
                j++;
            }
            array[curr] = Math.max(currans, array[curr]);
            ans = Math.max(ans, array[curr]);
        }
        return ans;
    }
}
```