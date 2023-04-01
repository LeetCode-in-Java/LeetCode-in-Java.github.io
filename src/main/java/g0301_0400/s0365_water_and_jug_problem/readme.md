[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 365\. Water and Jug Problem

Medium

You are given two jugs with capacities `jug1Capacity` and `jug2Capacity` liters. There is an infinite amount of water supply available. Determine whether it is possible to measure exactly `targetCapacity` liters using these two jugs.

If `targetCapacity` liters of water are measurable, you must have `targetCapacity` liters of water contained **within one or both buckets** by the end.

Operations allowed:

*   Fill any of the jugs with water.
*   Empty any of the jugs.
*   Pour water from one jug into another till the other jug is completely full, or the first jug itself is empty.

**Example 1:**

**Input:** jug1Capacity = 3, jug2Capacity = 5, targetCapacity = 4

**Output:** true

**Explanation:** The famous [Die Hard](https://www.youtube.com/watch?v=BVtQNK_ZUJg&ab_channel=notnek01) example

**Example 2:**

**Input:** jug1Capacity = 2, jug2Capacity = 6, targetCapacity = 5

**Output:** false

**Example 3:**

**Input:** jug1Capacity = 1, jug2Capacity = 2, targetCapacity = 3

**Output:** true

**Constraints:**

*   <code>1 <= jug1Capacity, jug2Capacity, targetCapacity <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    private int gcd(int n1, int n2) {
        if (n2 == 0) {
            return n1;
        }
        return gcd(n2, n1 % n2);
    }

    public boolean canMeasureWater(int jug1, int jug2, int target) {
        if (jug1 + jug2 < target) {
            return false;
        }
        int gcd = gcd(jug1, jug2);
        return target % gcd == 0;
    }
}
```