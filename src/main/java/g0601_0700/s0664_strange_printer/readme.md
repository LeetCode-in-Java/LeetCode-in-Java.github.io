## 664\. Strange Printer

Hard

There is a strange printer with the following two special properties:

*   The printer can only print a sequence of **the same character** each time.
*   At each turn, the printer can print new characters starting from and ending at any place and will cover the original existing characters.

Given a string `s`, return _the minimum number of turns the printer needed to print it_.

**Example 1:**

**Input:** s = "aaabbb"

**Output:** 2

**Explanation:** Print "aaa" first and then print "bbb".

**Example 2:**

**Input:** s = "aba"

**Output:** 2

**Explanation:** Print "aaa" first and then print "b" from the second place of the string, which will cover the existing character 'a'.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public int strangePrinter(String s) {
        if (s.length() == 0) {
            return 0;
        }
        int[][] dp = new int[s.length()][s.length()];
        for (int i = s.length() - 1; i >= 0; i--) {
            for (int j = i; j < s.length(); j++) {
                if (i == j) {
                    dp[i][j] = 1;
                } else if (s.charAt(i) == s.charAt(i + 1)) {
                    dp[i][j] = dp[i + 1][j];
                } else {
                    dp[i][j] = dp[i + 1][j] + 1;
                    for (int k = i + 1; k <= j; k++) {
                        if (s.charAt(k) == s.charAt(i)) {
                            dp[i][j] = Math.min(dp[i][j], dp[i + 1][k - 1] + dp[k][j]);
                        }
                    }
                }
            }
        }
        return dp[0][s.length() - 1];
    }
}
```