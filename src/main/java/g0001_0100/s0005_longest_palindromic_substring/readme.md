[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 5\. Longest Palindromic Substring

Medium

Given a string `s`, return _the longest palindromic substring_ in `s`.

**Example 1:**

**Input:** s = "babad"

**Output:** "bab" **Note:** "aba" is also a valid answer. 

**Example 2:**

**Input:** s = "cbbd"

**Output:** "bb" 

**Example 3:**

**Input:** s = "a"

**Output:** "a" 

**Example 4:**

**Input:** s = "ac"

**Output:** "a" 

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consist of only digits and English letters.

## Solution

```java
public class Solution {
    public String longestPalindrome(String s) {
        char[] newStr = new char[s.length() * 2 + 1];
        newStr[0] = '#';
        for (int i = 0; i < s.length(); i++) {
            newStr[2 * i + 1] = s.charAt(i);
            newStr[2 * i + 2] = '#';
        }
        int[] dp = new int[newStr.length];
        int friendCenter = 0;
        int friendRadius = 0;
        int lpsCenter = 0;
        int lpsRadius = 0;
        for (int i = 0; i < newStr.length; i++) {
            dp[i] =
                    friendCenter + friendRadius > i
                            ? Math.min(dp[friendCenter * 2 - i], (friendCenter + friendRadius) - i)
                            : 1;
            while (i + dp[i] < newStr.length
                    && i - dp[i] >= 0
                    && newStr[i + dp[i]] == newStr[i - dp[i]]) {
                dp[i]++;
            }
            if (friendCenter + friendRadius < i + dp[i]) {
                friendCenter = i;
                friendRadius = dp[i];
            }
            if (lpsRadius < dp[i]) {
                lpsCenter = i;
                lpsRadius = dp[i];
            }
        }
        return s.substring((lpsCenter - lpsRadius + 1) / 2, (lpsCenter + lpsRadius - 1) / 2);
    }
}
```