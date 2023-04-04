[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1542\. Find Longest Awesome Substring

Hard

You are given a string `s`. An **awesome** substring is a non-empty substring of `s` such that we can make any number of swaps in order to make it a palindrome.

Return _the length of the maximum length **awesome substring** of_ `s`.

**Example 1:**

**Input:** s = "3242415"

**Output:** 5

**Explanation:** "24241" is the longest awesome substring, we can form the palindrome "24142" with some swaps.

**Example 2:**

**Input:** s = "12345678"

**Output:** 1

**Example 3:**

**Input:** s = "213123"

**Output:** 6

**Explanation:** "213123" is the longest awesome substring, we can form the palindrome "231132" with some swaps.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists only of digits.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int longestAwesome(String s) {
        int n = s.length();
        int[] idx = new int[(int) Math.pow(2, 10)];
        Arrays.fill(idx, Integer.MAX_VALUE);
        idx[0] = -1;
        int mask = 0;
        int ans = 0;
        for (int i = 0; i < n; i++) {
            mask ^= 1 << (s.charAt(i) - '0');
            ans = Math.max(ans, i - idx[mask]);
            for (int j = 0; j < 10; j++) {
                ans = Math.max(ans, i - idx[mask ^ (1 << j)]);
            }
            idx[mask] = Math.min(idx[mask], i);
        }
        return ans;
    }
}
```