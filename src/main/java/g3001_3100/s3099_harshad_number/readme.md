[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3099\. Harshad Number

Easy

An integer divisible by the **sum** of its digits is said to be a **Harshad** number. You are given an integer `x`. Return _the sum of the digits_ of `x` if `x` is a **Harshad** number, otherwise, return `-1`_._

**Example 1:**

**Input:** x = 18

**Output:** 9

**Explanation:**

The sum of digits of `x` is `9`. `18` is divisible by `9`. So `18` is a Harshad number and the answer is `9`.

**Example 2:**

**Input:** x = 23

**Output:** \-1

**Explanation:**

The sum of digits of `x` is `5`. `23` is not divisible by `5`. So `23` is not a Harshad number and the answer is `-1`.

**Constraints:**

*   `1 <= x <= 100`

## Solution

```java
public class Solution {
    public int sumOfTheDigitsOfHarshadNumber(int x) {
        int sum = 0;
        int digit;
        int temp = x;
        while (temp != 0) {
            digit = temp % 10;
            sum = sum + digit;
            temp = temp / 10;
        }
        if (sum != 0 && x % sum == 0) {
            return sum;
        }
        return -1;
    }
}
```