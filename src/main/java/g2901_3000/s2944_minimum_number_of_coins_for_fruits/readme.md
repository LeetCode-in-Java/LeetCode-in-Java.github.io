[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2944\. Minimum Number of Coins for Fruits

Medium

You are at a fruit market with different types of exotic fruits on display.

You are given a **1-indexed** array `prices`, where `prices[i]` denotes the number of coins needed to purchase the <code>i<sup>th</sup></code> fruit.

The fruit market has the following offer:

*   If you purchase the <code>i<sup>th</sup></code> fruit at `prices[i]` coins, you can get the next `i` fruits for free.

**Note** that even if you **can** take fruit `j` for free, you can still purchase it for `prices[j]` coins to receive a new offer.

Return _the **minimum** number of coins needed to acquire all the fruits_.

**Example 1:**

**Input:** prices = [3,1,2]

**Output:** 4

**Explanation:** You can acquire the fruits as follows:

- Purchase the 1<sup>st</sup> fruit with 3 coins, you are allowed to take the 2<sup>nd</sup> fruit for free.

- Purchase the 2<sup>nd</sup> fruit with 1 coin, you are allowed to take the 3<sup>rd</sup> fruit for free.

- Take the 3<sup>rd</sup> fruit for free.

Note that even though you were allowed to take the 2<sup>nd</sup> fruit for free, you purchased it because it is more optimal.

It can be proven that 4 is the minimum number of coins needed to acquire all the fruits. 

**Example 2:**

**Input:** prices = [1,10,1,1]

**Output:** 2

**Explanation:** You can acquire the fruits as follows:

- Purchase the 1<sup>st</sup> fruit with 1 coin, you are allowed to take the 2<sup>nd</sup> fruit for free.

- Take the 2<sup>nd</sup> fruit for free.

- Purchase the 3<sup>rd</sup> fruit for 1 coin, you are allowed to take the 4<sup>th</sup> fruit for free.

- Take the 4<sup>t</sup><sup>h</sup> fruit for free.

It can be proven that 2 is the minimum number of coins needed to acquire all the fruits. 

**Constraints:**

*   `1 <= prices.length <= 1000`
*   <code>1 <= prices[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int minimumCoins(int[] prices) {
        int n = prices.length;
        int[] dp = new int[n];
        dp[n - 1] = prices[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            int pos = i + 1;
            int acquired = i + pos;
            if (acquired + 1 < n) {
                int min = Integer.MAX_VALUE;
                for (int j = acquired + 1; j >= i + 1; j--) {
                    min = Math.min(min, dp[j]);
                }
                dp[i] = prices[i] + min;
            } else {
                dp[i] = prices[i];
            }
        }
        return dp[0];
    }
}
```