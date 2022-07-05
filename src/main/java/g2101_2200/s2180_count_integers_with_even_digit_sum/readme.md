[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2180\. Count Integers With Even Digit Sum

Easy

Given a positive integer `num`, return _the number of positive integers **less than or equal to**_ `num` _whose digit sums are **even**_.

The **digit sum** of a positive integer is the sum of all its digits.

**Example 1:**

**Input:** num = 4

**Output:** 2

**Explanation:** 

The only integers less than or equal to 4 whose digit sums are even are 2 and 4.

**Example 2:**

**Input:** num = 30

**Output:** 14

**Explanation:** 

The 14 integers less than or equal to 30 whose digit sums are even are 

2, 4, 6, 8, 11, 13, 15, 17, 19, 20, 22, 24, 26, and 28.

**Constraints:**

* `1 <= num <= 1000`

## Solution

```java
public class Solution {
    public int countEven(int n) {
        if (n % 2 == 1) {
            return n / 2;
        } else {
            int ans = 0;
            int num = n;
            while (num != 0) {
                ans += num % 10;
                num /= 10;
            }
            if (ans % 2 == 0) {
                return n / 2;
            } else {
                return n / 2 - 1;
            }
        }
    }
}
```