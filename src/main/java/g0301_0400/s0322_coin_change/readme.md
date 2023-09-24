[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 322\. Coin Change

Medium

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**

**Input:** coins = [1,2,5], amount = 11

**Output:** 3

**Explanation:** 11 = 5 + 5 + 1 

**Example 2:**

**Input:** coins = [2], amount = 3

**Output:** -1 

**Example 3:**

**Input:** coins = [1], amount = 0

**Output:** 0 

**Constraints:**

*   `1 <= coins.length <= 12`
*   <code>1 <= coins[i] <= 2<sup>31</sup> - 1</code>
*   <code>0 <= amount <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                int prev = dp[i - coin];
                if (prev > 0) {
                    if (dp[i] == 0) {
                        dp[i] = prev + 1;
                    } else {
                        dp[i] = Math.min(dp[i], prev + 1);
                    }
                }
            }
        }
        return dp[amount] - 1;
    }
}
```

**Time Complexity (Big O Time):**

1. The program uses dynamic programming to compute the minimum number of coins needed for each possible amount from 0 to `amount`.
2. It iterates through the `coins` array, which has 'm' elements, and for each coin, it iterates from the coin value to `amount`, which has 'n' values.
3. Inside the inner loop, there is a constant-time operation to update the `dp` array.
4. Therefore, the overall time complexity of the program is O(m * n), where 'm' is the number of coin denominations, and 'n' is the given `amount`.

**Space Complexity (Big O Space):**

1. The program uses an integer array `dp` of size `amount + 1` to store the minimum number of coins needed for each amount from 0 to `amount`. Therefore, the space complexity is O(amount).

In summary, the provided program has a time complexity of O(m * n) and a space complexity of O(amount), where 'm' is the number of coin denominations, 'n' is the given `amount`, and the space complexity depends on the value of `amount`.
