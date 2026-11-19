[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4021\. Minimum Operations to Make a Rotated Palindrome I

Medium

You are given a string `s` consisting of lowercase English letters.

You can perform the following operations any number of times (including zero) and in any order:

*   **Increment**: Choose any index `i` and replace `s[i]` with the next lowercase English letter. The letter after `'z'` is `'a'`.
*   **Left rotate**: Move the first character of the string to the end.

Return the **minimum** number of operations required to make `s` a palindrome.

**Example 1:**

**Input:** s = "abc"

**Output:** 2

**Explanation:**

One optimal solution:

*   Left rotate the string: `"abc" -> "bca"`.
*   Increment `'a'` to `'b'`: `"bca" -> "bcb"`.
*   `"bcb"` is a palindrome. Thus, the answer is 2.

**Example 2:**

**Input:** s = "yb"

**Output:** 3

**Explanation:**

*   Increment the first character three times: `"yb" -> "zb" -> "ab" -> "bb"`.
*   `"bb"` is a palindrome. Thus, the answer is 3.

**Constraints:**

*   `2 <= s.length <= 2000`
*   `s` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public int minOperations(String s) {
        char[] chars = s.toCharArray();
        int n = s.length();
        int[][] dist = new int[26][26];
        for (int a = 0; a < 26; ++a) {
            for (int b = 0; b < 26; ++b) {
                int diff = Math.abs(a - b);
                dist[a][b] = Math.min(diff, 26 - diff);
            }
        }
        int[] vals = new int[n];
        for (int i = 0; i < n; ++i) {
            vals[i] = chars[i] - 97;
        }
        int mid = n >> 1;
        int min = 0;
        for (int i = 0; i < mid; ++i) {
            min += dist[vals[i]][vals[n - 1 - i]];
        }
        int[] diff = new int[n << 1];
        for (int i = 0; i < 2 * n; ++i) {
            diff[i] = vals[i % n];
        }
        for (int shift = 1; shift < n >> (1 - (n & 1)) && shift < min; ++shift) {
            int curr = shift;
            for (int i = 0; i < mid; ++i) {
                curr += dist[diff[shift + i]][diff[shift + n - 1 - i]];
                if (curr >= min) {
                    break;
                }
            }
            min = Math.min(min, curr);
        }
        return min;
    }
}
```