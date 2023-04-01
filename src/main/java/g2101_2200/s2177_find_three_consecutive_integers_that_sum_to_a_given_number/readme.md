[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2177\. Find Three Consecutive Integers That Sum to a Given Number

Medium

Given an integer `num`, return _three consecutive integers (as a sorted array)_ _that **sum** to_ `num`. If `num` cannot be expressed as the sum of three consecutive integers, return _an **empty** array._

**Example 1:**

**Input:** num = 33

**Output:** [10,11,12]

**Explanation:** 33 can be expressed as 10 + 11 + 12 = 33. 

10, 11, 12 are 3 consecutive integers, so we return [10, 11, 12]. 

**Example 2:**

**Input:** num = 4

**Output:** []

**Explanation:** There is no way to express 4 as the sum of 3 consecutive integers. 

**Constraints:**

*   <code>0 <= num <= 10<sup>15</sup></code>

## Solution

```java
public class Solution {
    public long[] sumOfThree(long num) {
        if (num % 3 == 0) {
            return new long[] {num / 3 - 1, num / 3, num / 3 + 1};
        }
        return new long[0];
    }
}
```