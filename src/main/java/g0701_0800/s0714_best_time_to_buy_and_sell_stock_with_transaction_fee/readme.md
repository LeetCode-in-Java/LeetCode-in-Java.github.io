[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 714\. Best Time to Buy and Sell Stock with Transaction Fee

Medium

You are given an array `prices` where `prices[i]` is the price of a given stock on the <code>i<sup>th</sup></code> day, and an integer `fee` representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** prices = [1,3,2,8,4,9], fee = 2

**Output:** 8

**Explanation:** The maximum profit can be achieved by: 

- Buying at prices[0] = 1 

- Selling at prices[3] = 8 

- Buying at prices[4] = 4 

- Selling at prices[5] = 9 
  
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.

**Example 2:**

**Input:** prices = [1,3,7,5,10,3], fee = 3

**Output:** 6

**Constraints:**

*   <code>1 <= prices.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= prices[i] < 5 * 10<sup>4</sup></code>
*   <code>0 <= fee < 5 * 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int maxProfit(int[] prices, int fee) {
        int cash = 0;
        int hold = -prices[0];
        for (int i = 1; i < prices.length; i++) {
            cash = Math.max(cash, hold + prices[i] - fee);
            hold = Math.max(hold, cash - prices[i]);
        }
        return cash;
    }
}
```