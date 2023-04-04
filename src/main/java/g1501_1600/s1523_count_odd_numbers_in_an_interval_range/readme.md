[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1523\. Count Odd Numbers in an Interval Range

Easy

Given two non-negative integers `low` and `high`. Return the _count of odd numbers between_ `low` _and_ `high`_ (inclusive)_.

**Example 1:**

**Input:** low = 3, high = 7

**Output:** 3

**Explanation:** The odd numbers between 3 and 7 are [3,5,7].

**Example 2:**

**Input:** low = 8, high = 10

**Output:** 1

**Explanation:** The odd numbers between 8 and 10 are [9].

**Constraints:**

*   `0 <= low <= high <= 10^9`

## Solution

```java
public class Solution {
    public int countOdds(int low, int high) {
        if (low % 2 != 0 || high % 2 != 0) {
            return (high - low) / 2 + 1;
        }
        return (high - low) / 2;
    }
}
```