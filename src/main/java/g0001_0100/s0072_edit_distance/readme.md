## 72\. Edit Distance

Hard

Given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:

*   Insert a character
*   Delete a character
*   Replace a character

**Example 1:**

**Input:** word1 = "horse", word2 = "ros"

**Output:** 3

**Explanation:** horse -> rorse (replace 'h' with 'r') rorse -> rose (remove 'r') rose -> ros (remove 'e') 

**Example 2:**

**Input:** word1 = "intention", word2 = "execution"

**Output:** 5

**Explanation:** intention -> inention (remove 't') inention -> enention (replace 'i' with 'e') enention -> exention (replace 'n' with 'x') exention -> exection (replace 'n' with 'c') exection -> execution (insert 'u') 

**Constraints:**

*   `0 <= word1.length, word2.length <= 500`
*   `word1` and `word2` consist of lowercase English letters.

## Solution

```java
@SuppressWarnings("java:S2234")
public class Solution {
    public int minDistance(String w1, String w2) {
        int n1 = w1.length();
        int n2 = w2.length();
        if (n2 > n1) {
            return minDistance(w2, w1);
        }
        int[] dp = new int[n2 + 1];
        for (int j = 0; j <= n2; j++) {
            dp[j] = j;
        }
        for (int i = 1; i <= n1; i++) {
            int pre = dp[0];
            dp[0] = i;
            for (int j = 1; j <= n2; j++) {
                int tmp = dp[j];
                dp[j] =
                        w1.charAt(i - 1) != w2.charAt(j - 1)
                                ? 1 + Math.min(pre, Math.min(dp[j], dp[j - 1]))
                                : pre;
                pre = tmp;
            }
        }
        return dp[n2];
    }
}
```