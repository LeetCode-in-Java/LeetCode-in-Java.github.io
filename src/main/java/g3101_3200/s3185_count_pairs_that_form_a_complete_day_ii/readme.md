[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3185\. Count Pairs That Form a Complete Day II

Medium

Given an integer array `hours` representing times in **hours**, return an integer denoting the number of pairs `i`, `j` where `i < j` and `hours[i] + hours[j]` forms a **complete day**.

A **complete day** is defined as a time duration that is an **exact** **multiple** of 24 hours.

For example, 1 day is 24 hours, 2 days is 48 hours, 3 days is 72 hours, and so on.

**Example 1:**

**Input:** hours = [12,12,30,24,24]

**Output:** 2

**Explanation:** The pairs of indices that form a complete day are `(0, 1)` and `(3, 4)`.

**Example 2:**

**Input:** hours = [72,48,24,3]

**Output:** 3

**Explanation:** The pairs of indices that form a complete day are `(0, 1)`, `(0, 2)`, and `(1, 2)`.

**Constraints:**

*   <code>1 <= hours.length <= 5 * 10<sup>5</sup></code>
*   <code>1 <= hours[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public long countCompleteDayPairs(int[] hours) {
        long[] hour = new long[24];
        for (int j : hours) {
            hour[j % 24]++;
        }
        long counter = hour[0] * (hour[0] - 1) / 2;
        counter += hour[12] * (hour[12] - 1) / 2;
        for (int i = 1; i < 12; i++) {
            counter += hour[i] * hour[24 - i];
        }
        return counter;
    }
}
```