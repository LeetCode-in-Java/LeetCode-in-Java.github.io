[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1281\. Subtract the Product and Sum of Digits of an Integer

Easy

Given an integer number `n`, return the difference between the product of its digits and the sum of its digits.

**Example 1:**

**Input:** n = 234

**Output:** 15

**Explanation:**

Product of digits = 2 \* 3 \* 4 = 24

Sum of digits = 2 + 3 + 4 = 9

Result = 24 - 9 = 15

**Example 2:**

**Input:** n = 4421

**Output:** 21

**Explanation:**

Product of digits = 4 \* 4 \* 2 \* 1 = 32

Sum of digits = 4 + 4 + 2 + 1 = 11

Result = 32 - 11 = 21

**Constraints:**

*   `1 <= n <= 10^5`

## Solution

```java
public class Solution {
    public int subtractProductAndSum(int n) {
        int product = 1;
        int sum = 0;
        while (n != 0) {
            int digit = n % 10;
            product = product * digit;
            sum = sum + digit;
            n /= 10;
        }
        return product - sum;
    }
}
```