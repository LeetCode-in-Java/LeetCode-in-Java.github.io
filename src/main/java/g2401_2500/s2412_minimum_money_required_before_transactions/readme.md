[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2412\. Minimum Money Required Before Transactions

Hard

You are given a **0-indexed** 2D integer array `transactions`, where <code>transactions[i] = [cost<sub>i</sub>, cashback<sub>i</sub>]</code>.

The array describes transactions, where each transaction must be completed exactly once in **some order**. At any given moment, you have a certain amount of `money`. In order to complete transaction `i`, <code>money >= cost<sub>i</sub></code> must hold true. After performing a transaction, `money` becomes <code>money - cost<sub>i</sub> + cashback<sub>i</sub></code>.

Return _the minimum amount of_ `money` _required before any transaction so that all of the transactions can be completed **regardless of the order** of the transactions._

**Example 1:**

**Input:** transactions = \[\[2,1],[5,0],[4,2]]

**Output:** 10

**Explanation:**

Starting with money = 10, the transactions can be performed in any order.

It can be shown that starting with money < 10 will fail to complete all transactions in some order. 

**Example 2:**

**Input:** transactions = \[\[3,0],[0,3]]

**Output:** 3

**Explanation:**

- If transactions are in the order [[3,0],[0,3]], the minimum money required to complete the transactions is 3.

- If transactions are in the order [[0,3],[3,0]], the minimum money required to complete the transactions is 0.

Thus, starting with money = 3, the transactions can be performed in any order. 

**Constraints:**

*   <code>1 <= transactions.length <= 10<sup>5</sup></code>
*   `transactions[i].length == 2`
*   <code>0 <= cost<sub>i</sub>, cashback<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public long minimumMoney(int[][] transactions) {
        int max = 0;
        long sum = 0;
        for (int[] transaction : transactions) {
            int diff = transaction[1] - transaction[0];
            if (diff < 0) {
                sum -= diff;
                transaction[0] += diff;
            }
            max = Math.max(max, transaction[0]);
        }
        return max + sum;
    }
}
```