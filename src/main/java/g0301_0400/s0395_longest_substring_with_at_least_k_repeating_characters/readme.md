## 395\. Longest Substring with At Least K Repeating Characters

Medium

Given a string `s` and an integer `k`, return _the length of the longest substring of_ `s` _such that the frequency of each character in this substring is greater than or equal to_ `k`.

**Example 1:**

**Input:** s = "aaabb", k = 3

**Output:** 3

**Explanation:** The longest substring is "aaa", as 'a' is repeated 3 times.

**Example 2:**

**Input:** s = "ababbc", k = 2

**Output:** 5

**Explanation:** The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of only lowercase English letters.
*   <code>1 <= k <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int longestSubstring(String s, int k) {
        return helper(s, k, 0, s.length());
    }

    private int helper(String s, int k, int start, int end) {
        if (end - start < k) {
            return 0;
        }
        int[] nums = new int[26];
        for (int i = start; i < end; i++) {
            nums[s.charAt(i) - 'a']++;
        }

        for (int i = start; i < end; i++) {
            if (nums[s.charAt(i) - 'a'] < k) {
                int j = i + 1;
                while (j < s.length() && nums[s.charAt(j) - 'a'] < k) {
                    j++;
                }
                return Math.max(helper(s, k, start, i), helper(s, k, j, end));
            }
        }
        return end - start;
    }
}
```