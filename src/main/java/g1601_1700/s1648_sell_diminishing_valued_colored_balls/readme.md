[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1648\. Sell Diminishing-Valued Colored Balls

Medium

You have an `inventory` of different colored balls, and there is a customer that wants `orders` balls of **any** color.

The customer weirdly values the colored balls. Each colored ball's value is the number of balls **of that color **you currently have in your `inventory`. For example, if you own `6` yellow balls, the customer would pay `6` for the first yellow ball. After the transaction, there are only `5` yellow balls left, so the next yellow ball is then valued at `5` (i.e., the value of the balls decreases as you sell more to the customer).

You are given an integer array, `inventory`, where `inventory[i]` represents the number of balls of the <code>i<sup>th</sup></code> color that you initially own. You are also given an integer `orders`, which represents the total number of balls that the customer wants. You can sell the balls **in any order**.

Return _the **maximum** total value that you can attain after selling_ `orders` _colored balls_. As the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/05/jj.gif)

**Input:** inventory = [2,5], orders = 4

**Output:** 14

**Explanation:** Sell the 1st color 1 time (2) and the 2nd color 3 times (5 + 4 + 3). The maximum total value is 2 + 5 + 4 + 3 = 14.

**Example 2:**

**Input:** inventory = [3,5], orders = 6

**Output:** 19

**Explanation:** Sell the 1st color 2 times (3 + 2) and the 2nd color 4 times (5 + 4 + 3 + 2). The maximum total value is 3 + 2 + 5 + 4 + 3 + 2 = 19.

**Constraints:**

*   <code>1 <= inventory.length <= 10<sup>5</sup></code>
*   <code>1 <= inventory[i] <= 10<sup>9</sup></code>
*   <code>1 <= orders <= min(sum(inventory[i]), 10<sup>9</sup>)</code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxProfit(int[] inventory, int orders) {
        int n = inventory.length;
        long mod = (long) 1e9 + 7;
        long totalValue = 0;
        Arrays.sort(inventory);
        int count = 0;
        for (int i = n - 1; i >= 0; i--) {
            count++;
            if (i == 0 || inventory[i] > inventory[i - 1]) {
                long diff = i == 0 ? inventory[i] : inventory[i] - inventory[i - 1];
                if (count * diff < orders) {
                    totalValue += (2L * inventory[i] - diff + 1) * diff * count / 2 % mod;
                    orders -= count * diff;
                } else {
                    diff = orders / count;
                    long remainder = orders % count;
                    totalValue += (2L * inventory[i] - diff + 1) * diff * count / 2 % mod;
                    totalValue += (inventory[i] - diff) * remainder % mod;
                    totalValue %= mod;
                    break;
                }
            }
        }
        return (int) totalValue;
    }
}
```