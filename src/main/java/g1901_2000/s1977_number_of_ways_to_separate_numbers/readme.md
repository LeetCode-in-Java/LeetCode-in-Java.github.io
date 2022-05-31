## 1977\. Number of Ways to Separate Numbers

Hard

You wrote down many **positive** integers in a string called `num`. However, you realized that you forgot to add commas to seperate the different numbers. You remember that the list of integers was **non-decreasing** and that **no** integer had leading zeros.

Return _the **number of possible lists of integers** that you could have written down to get the string_ `num`. Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** num = "327"

**Output:** 2

**Explanation:** You could have written down the numbers: 3, 27 327

**Example 2:**

**Input:** num = "094"

**Output:** 0

**Explanation:** No numbers can have leading zeros and all numbers must be positive.

**Example 3:**

**Input:** num = "0"

**Output:** 0

**Explanation:** No numbers can have leading zeros and all numbers must be positive.

**Constraints:**

*   `1 <= num.length <= 3500`
*   `num` consists of digits `'0'` through `'9'`.

## Solution

```java
public class Solution {
    private int[][] lcp;
    private long[][] dp;
    private long[][] dps;
    private String num;
    private int n;

    // Pre-compute The Longest Common Prefix sequence for each index in the string
    private void calcLCP() {
        for (int i = n - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (num.charAt(i) == num.charAt(j)) {
                    lcp[i][j] = lcp[i + 1][j + 1] + 1;
                }
            }
        }
    }

    // compare substring of same length for value
    private boolean compare(int i, int j, int len) {
        int common = lcp[i][j];
        return common >= len || num.charAt(i + common) < num.charAt(j + common);
    }

    // calculates number of possible separations
    private void calcResult() {
        for (int i = n - 1; i >= 0; i--) {
            // leading zero at current current index
            if (num.charAt(i) == '0') {
                continue;
            }
            // for substring starting at index i
            long sum = 0;
            for (int j = n - 1; j >= i; j--) {
                long mod = 1000000007;
                if (j == n - 1) {
                    // whole substring from index i is a valid possible list of integer (single
                    // integer in this case)
                    dp[i][j] = 1;
                } else {
                    // first integer (i-j)
                    int len = j - i + 1;
                    // second integer start index
                    int st = j + 1;
                    // second integer end index
                    int ed = st + len - 1;
                    // equal length integers should be compared for value
                    dp[i][j] = 0;
                    if (ed < n && compare(i, st, len)) {
                        dp[i][j] = dp[st][ed];
                    }
                    // including the second integers possibilities with length greater than 1st one.
                    if (ed + 1 < n) {
                        // dps[st][ed+1] => dp[st][ed+1].......dp[st][n-1]
                        dp[i][j] = (dp[i][j] + dps[st][ed + 1]) % mod;
                    }
                }
                dps[i][j] = sum = (sum + dp[i][j]) % mod;
            }
        }
    }

    public int numberOfCombinations(String num) {
        if (num.charAt(0) == '0') {
            return 0;
        }
        n = num.length();
        this.num = num;
        lcp = new int[n + 1][n + 1];
        dp = new long[n + 1][n + 1];
        dps = new long[n + 1][n + 1];
        calcLCP();
        calcResult();
        return (int) dps[0][0];
    }
}
```