[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1672\. Richest Customer Wealth

Easy

You are given an `m x n` integer grid `accounts` where `accounts[i][j]` is the amount of money the <code>i<sup>th</sup></code> customer has in the <code>j<sup>th</sup></code> bank. Return _the **wealth** that the richest customer has._

A customer's **wealth** is the amount of money they have in all their bank accounts. The richest customer is the customer that has the maximum **wealth**.

**Example 1:**

**Input:** accounts = \[\[1,2,3],[3,2,1]]

**Output:** 6

**Explanation::**

`1st customer has wealth = 1 + 2 + 3 = 6`

`2nd customer has wealth = 3 + 2 + 1 = 6`

Both customers are considered the richest with a wealth of 6 each, so return 6. 

**Example 2:**

**Input:** accounts = \[\[1,5],[7,3],[3,5]]

**Output:** 10

**Explanation:**

1st customer has wealth = 6

2nd customer has wealth = 10

3rd customer has wealth = 8

The 2nd customer is the richest with a wealth of 10.

**Example 3:**

**Input:** accounts = \[\[2,8,7],[7,1,3],[1,9,5]]

**Output:** 17 

**Constraints:**

*   `m == accounts.length`
*   `n == accounts[i].length`
*   `1 <= m, n <= 50`
*   `1 <= accounts[i][j] <= 100`

## Solution

```java
public class Solution {
    public int maximumWealth(int[][] accounts) {
        int max = Integer.MIN_VALUE;
        for (int[] account : accounts) {
            int sum = 0;
            for (int i : account) {
                sum += i;
            }
            max = Math.max(max, sum);
        }
        return max;
    }
}
```