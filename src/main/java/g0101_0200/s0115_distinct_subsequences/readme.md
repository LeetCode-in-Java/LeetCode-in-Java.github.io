## 115\. Distinct Subsequences

Hard

Given two strings `s` and `t`, return _the number of distinct subsequences of `s` which equals `t`_.

A string's **subsequence** is a new string formed from the original string by deleting some (can be none) of the characters without disturbing the remaining characters' relative positions. (i.e., `"ACE"` is a subsequence of `"ABCDE"` while `"AEC"` is not).

It is guaranteed the answer fits on a 32-bit signed integer.

**Example 1:**

**Input:** s = "rabbbit", t = "rabbit"

**Output:** 3

**Explanation:** As shown below, there are 3 ways you can generate "rabbit" from S. `**rabb**b**it**` `**ra**b**bbit**` `**rab**b**bit**` 

**Example 2:**

**Input:** s = "babgbag", t = "bag"

**Output:** 5

**Explanation:** As shown below, there are 5 ways you can generate "bag" from S. `**ba**b**g**bag` `**ba**bgba**g**` `**b**abgb**ag**` `ba**b**gb**ag**` `babg**bag**`

**Constraints:**

*   `1 <= s.length, t.length <= 1000`
*   `s` and `t` consist of English letters.

## Solution

```java
public class Solution {
    public int numDistinct(String text, String text2) {
        if (text.length() < text2.length()) {
            return 0;
        }
        if (text.length() == text2.length()) {
            return (text.equals(text2) ? 1 : 0);
        }
        int move = text.length() - text2.length() + 2;
        // Only finite number of character in s can occupy first position in T. Same applies for
        // every character in T.
        int[] dp = new int[move];
        int j = 1;
        int k = 1;
        for (int i = 0; i < text2.length(); i++) {
            boolean firstMatch = true;
            for (; j < move; j++) {
                if (text2.charAt(i) == text.charAt(i + j - 1)) {
                    if (firstMatch) {
                        // Keep track of first match. To avoid useless comparisons on next
                        // iteration.
                        k = j;
                        firstMatch = false;
                    }
                    if (i == 0) {
                        dp[j] = 1;
                    }
                    dp[j] += dp[j - 1];
                } else {
                    dp[j] = dp[j - 1];
                }
            }
            // No match found for current character of t in s. No point in checking others.
            if (dp[move - 1] == 0) {
                return 0;
            }
            j = k;
        }
        return dp[move - 1];
    }
}
```