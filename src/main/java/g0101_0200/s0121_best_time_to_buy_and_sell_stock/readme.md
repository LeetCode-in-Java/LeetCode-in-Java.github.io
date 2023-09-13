[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 121\. Best Time to Buy and Sell Stock

Easy

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

**Example 1:**

**Input:** prices = [7,1,5,3,6,4]

**Output:** 5

**Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5. Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell. 

**Example 2:**

**Input:** prices = [7,6,4,3,1]

**Output:** 0

**Explanation:** In this case, no transactions are done and the max profit = 0. 

**Constraints:**

*   <code>1 <= prices.length <= 10<sup>5</sup></code>
*   <code>0 <= prices[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        int min = prices[0];
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > min) {
                maxProfit = Math.max(maxProfit, prices[i] - min);
            } else {
                min = prices[i];
            }
        }
        return maxProfit;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of the program is determined by the loop that iterates through the array of stock prices. Here's the breakdown:

- The loop starts from the second element (index 1) and goes up to the last element (index `prices.length - 1`).

- Inside the loop, there are constant-time operations such as comparisons and arithmetic operations.

- The key operation is the `Math.max` function, which calculates the maximum of two values. This operation is also considered a constant-time operation.

- Therefore, the overall time complexity of the program is O(N), where N is the number of elements in the `prices` array. It needs to iterate through the array once.

**Space Complexity (Big O Space):**

The space complexity of the program is relatively low because it doesn't use any additional data structures that grow with the input size. Here's the analysis:

- The program uses a fixed number of integer variables (`maxProfit` and `min`) that occupy constant space, regardless of the input size.

- It doesn't use any dynamically allocated memory or data structures that depend on the input size.

- Therefore, the space complexity of the program is O(1), indicating constant space usage.

In summary, the time complexity of the program is O(N), where N is the number of stock prices, and the space complexity is O(1), indicating constant space usage. The program efficiently finds the maximum profit by iterating through the stock prices array once.
