[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 424\. Longest Repeating Character Replacement

Medium

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return _the length of the longest substring containing the same letter you can get after performing the above operations_.

**Example 1:**

**Input:** s = "ABAB", k = 2

**Output:** 4

**Explanation:** Replace the two 'A's with two 'B's or vice versa. 

**Example 2:**

**Input:** s = "AABABBA", k = 1

**Output:** 4

**Explanation:** Replace the one 'A' in the middle with 'B' and form "AABBBBA". The substring "BBBB" has the longest repeating letters, which is 4. 

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only uppercase English letters.
*   `0 <= k <= s.length

## Solution

```java
public class Solution {
    public int characterReplacement(String s, int k) {
        int left = 0;
        int right = 0;
        int len = s.length();
        int[] count = new int[256];
        char[] sArr = s.toCharArray();
        int currMax = 0;
        int maxLen = 0;
        char curr;
        while (right < len) {
            curr = sArr[right];
            count[curr]++;
            currMax = Math.max(currMax, count[curr]);
            if (right - left + 1 <= currMax + k) {
                maxLen = Math.max(maxLen, right - left + 1);
            }
            while (right - left + 1 > currMax + k) {
                curr = sArr[left];
                count[curr]--;
                left++;
            }
            right++;
        }
        return maxLen;
    }
}
```