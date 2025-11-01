[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 123\. Best Time to Buy and Sell Stock III

Hard

You are given an array `prices` where `prices[i]` is the price of a given stock on the <code>i<sup>th</sup></code> day.

Find the maximum profit you can achieve. You may complete **at most two transactions**.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** prices = [3,3,5,0,0,3,1,4]

**Output:** 6

**Explanation:** Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3. Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.

**Example 2:**

**Input:** prices = [1,2,3,4,5]

**Output:** 4

**Explanation:** Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4. Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again. 

**Example 3:**

**Input:** prices = [7,6,4,3,1]

**Output:** 0

**Explanation:** In this case, no transaction is done, i.e. max profit = 0. 

**Constraints:**

*   <code>1 <= prices.length <= 10<sup>5</sup></code>
*   <code>0 <= prices[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length == 0) {
            return 0;
        }
        int fb = Integer.MIN_VALUE;
        int sb = Integer.MIN_VALUE;
        int fs = 0;
        int ss = 0;
        for (int price : prices) {
            fb = Math.max(fb, -price);
            fs = Math.max(fs, fb + price);
            sb = Math.max(sb, fs - price);
            ss = Math.max(ss, sb + price);
        }
        return ss;
    }
}
```