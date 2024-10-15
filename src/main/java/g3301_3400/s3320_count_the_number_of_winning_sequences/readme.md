[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3320\. Count The Number of Winning Sequences

Hard

Alice and Bob are playing a fantasy battle game consisting of `n` rounds where they summon one of three magical creatures each round: a Fire Dragon, a Water Serpent, or an Earth Golem. In each round, players **simultaneously** summon their creature and are awarded points as follows:

*   If one player summons a Fire Dragon and the other summons an Earth Golem, the player who summoned the **Fire Dragon** is awarded a point.
*   If one player summons a Water Serpent and the other summons a Fire Dragon, the player who summoned the **Water Serpent** is awarded a point.
*   If one player summons an Earth Golem and the other summons a Water Serpent, the player who summoned the **Earth Golem** is awarded a point.
*   If both players summon the same creature, no player is awarded a point.

You are given a string `s` consisting of `n` characters `'F'`, `'W'`, and `'E'`, representing the sequence of creatures Alice will summon in each round:

*   If `s[i] == 'F'`, Alice summons a Fire Dragon.
*   If `s[i] == 'W'`, Alice summons a Water Serpent.
*   If `s[i] == 'E'`, Alice summons an Earth Golem.

Bob’s sequence of moves is unknown, but it is guaranteed that Bob will never summon the same creature in two consecutive rounds. Bob _beats_ Alice if the total number of points awarded to Bob after `n` rounds is **strictly greater** than the points awarded to Alice.

Return the number of distinct sequences Bob can use to beat Alice.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "FFF"

**Output:** 3

**Explanation:**

Bob can beat Alice by making one of the following sequences of moves: `"WFW"`, `"FWF"`, or `"WEW"`. Note that other winning sequences like `"WWE"` or `"EWW"` are invalid since Bob cannot make the same move twice in a row.

**Example 2:**

**Input:** s = "FWEFW"

**Output:** 18

**Explanation:**

Bob can beat Alice by making one of the following sequences of moves: `"FWFWF"`, `"FWFWE"`, `"FWEFE"`, `"FWEWE"`, `"FEFWF"`, `"FEFWE"`, `"FEFEW"`, `"FEWFE"`, `"WFEFE"`, `"WFEWE"`, `"WEFWF"`, `"WEFWE"`, `"WEFEF"`, `"WEFEW"`, `"WEWFW"`, `"WEWFE"`, `"EWFWE"`, or `"EWEWE"`.

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s[i]` is one of `'F'`, `'W'`, or `'E'`.

## Solution

```java
public class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int countWinningSequences(String s) {
        int n = s.length();
        int[][][] dp = new int[n][3][2 * n + 1];
        if (s.charAt(0) == 'F') {
            dp[0][0][n] = 1;
            dp[0][1][1 + n] = 1;
            dp[0][2][-1 + n] = 1;
        } else if (s.charAt(0) == 'W') {
            dp[0][0][-1 + n] = 1;
            dp[0][1][n] = 1;
            dp[0][2][1 + n] = 1;
        } else if (s.charAt(0) == 'E') {
            dp[0][0][1 + n] = 1;
            dp[0][1][-1 + n] = 1;
            dp[0][2][n] = 1;
        }
        for (int i = 1; i < n; i++) {
            if (s.charAt(i) == 'F') {
                for (int j = 0; j < 2 * n + 1; j++) {
                    dp[i][0][j] = (dp[i - 1][1][j] + dp[i - 1][2][j]) % MOD;
                }
                for (int j = 1; j < 2 * n + 1; j++) {
                    dp[i][1][j] = (dp[i - 1][0][j - 1] + dp[i - 1][2][j - 1]) % MOD;
                }
                for (int j = 0; j < 2 * n; j++) {
                    dp[i][2][j] = (dp[i - 1][0][j + 1] + dp[i - 1][1][j + 1]) % MOD;
                }
            } else if (s.charAt(i) == 'W') {
                for (int j = 0; j < 2 * n + 1; j++) {
                    dp[i][1][j] = (dp[i - 1][0][j] + dp[i - 1][2][j]) % MOD;
                }
                for (int j = 1; j < 2 * n + 1; j++) {
                    dp[i][2][j] = (dp[i - 1][0][j - 1] + dp[i - 1][1][j - 1]) % MOD;
                }
                for (int j = 0; j < 2 * n; j++) {
                    dp[i][0][j] = (dp[i - 1][1][j + 1] + dp[i - 1][2][j + 1]) % MOD;
                }
            } else if (s.charAt(i) == 'E') {
                for (int j = 0; j < 2 * n; j++) {
                    dp[i][2][j] = (dp[i - 1][0][j] + dp[i - 1][1][j]) % MOD;
                }
                for (int j = 1; j < 2 * n + 1; j++) {
                    dp[i][0][j] = (dp[i - 1][1][j - 1] + dp[i - 1][2][j - 1]) % MOD;
                }
                for (int j = 0; j < 2 * n; j++) {
                    dp[i][1][j] = (dp[i - 1][0][j + 1] + dp[i - 1][2][j + 1]) % MOD;
                }
            }
        }
        int count = 0;
        for (int j = n + 1; j < 2 * n + 1; j++) {
            count = (count + dp[n - 1][0][j]) % MOD;
            count = (count + dp[n - 1][1][j]) % MOD;
            count = (count + dp[n - 1][2][j]) % MOD;
        }
        return count % MOD;
    }
}
```