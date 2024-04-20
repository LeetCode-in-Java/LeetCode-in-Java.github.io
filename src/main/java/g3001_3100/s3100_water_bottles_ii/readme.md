[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3100\. Water Bottles II

Medium

You are given two integers `numBottles` and `numExchange`.

`numBottles` represents the number of full water bottles that you initially have. In one operation, you can perform one of the following operations:

*   Drink any number of full water bottles turning them into empty bottles.
*   Exchange `numExchange` empty bottles with one full water bottle. Then, increase `numExchange` by one.

Note that you cannot exchange multiple batches of empty bottles for the same value of `numExchange`. For example, if `numBottles == 3` and `numExchange == 1`, you cannot exchange `3` empty water bottles for `3` full bottles.

Return _the **maximum** number of water bottles you can drink_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2024/01/28/exampleone1.png)

**Input:** numBottles = 13, numExchange = 6

**Output:** 15

**Explanation:** The table above shows the number of full water bottles, empty water bottles, the value of numExchange, and the number of bottles drunk.

**Example 2:**

![](https://assets.leetcode.com/uploads/2024/01/28/example231.png)

**Input:** numBottles = 10, numExchange = 3

**Output:** 13

**Explanation:** The table above shows the number of full water bottles, empty water bottles, the value of numExchange, and the number of bottles drunk.

**Constraints:**

*   `1 <= numBottles <= 100`
*   `1 <= numExchange <= 100`

## Solution

```java
public class Solution {
    public int maxBottlesDrunk(int numBottles, int numExchange) {
        int emptyBottles = numBottles;
        int bottleDrinks = numBottles;
        while (numExchange <= emptyBottles) {
            bottleDrinks += 1;
            emptyBottles = 1 + (emptyBottles - numExchange);
            numExchange++;
        }
        return bottleDrinks;
    }
}
```