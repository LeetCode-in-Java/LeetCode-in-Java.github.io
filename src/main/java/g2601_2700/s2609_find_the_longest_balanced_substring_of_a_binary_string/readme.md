[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2609\. Find the Longest Balanced Substring of a Binary String

Easy

You are given a binary string `s` consisting only of zeroes and ones.

A substring of `s` is considered balanced if **all zeroes are before ones** and the number of zeroes is equal to the number of ones inside the substring. Notice that the empty substring is considered a balanced substring.

Return _the length of the longest balanced substring of_ `s`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "01000111"

**Output:** 6

**Explanation:** The longest balanced substring is "000111", which has length 6.

**Example 2:**

**Input:** s = "00111"

**Output:** 4

**Explanation:** The longest balanced substring is "0011", which has length 4.

**Example 3:**

**Input:** s = "111"

**Output:** 0

**Explanation:** There is no balanced substring except the empty substring, so the answer is 0.

**Constraints:**

*   `1 <= s.length <= 50`
*   `'0' <= s[i] <= '1'`

## Solution

```java
public class Solution {
    public int findTheLongestBalancedSubstring(String s) {
        char[] chars = s.toCharArray();
        int max = 0;
        int n = chars.length;
        int zero = 0;
        int one = 0;
        int i = 0;
        while (i < n) {
            if (chars[i] == '0') {
                zero++;
            } else {
                while (i < n) {
                    if (chars[i++] == '1') {
                        one++;
                    } else {
                        i--;
                        break;
                    }
                }
                max = Math.max(max, 2 * Math.min(one, zero));
                zero = 1;
                one = 0;
            }
            i++;
        }

        return max;
    }
}
```