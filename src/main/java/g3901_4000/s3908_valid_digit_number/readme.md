[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3908\. Valid Digit Number

Easy

You are given an integer `n` and a digit `x`.

A number is considered **valid** if:

*   It contains **at least one** occurrence of digit `x`, and
*   It **does not start** with digit `x`.

Return `true` if `n` is **valid**, otherwise return `false`.

**Example 1:**

**Input:** n = 101, x = 0

**Output:** true

**Explanation:**

The number contains digit 0 at index 1. It does not start with 0, so it satisfies both conditions. Thus, the answer is `true`.

**Example 2:**

**Input:** n = 232, x = 2

**Output:** false

**Explanation:**

The number starts with 2, which violates the condition. Thus, the answer is `false`.

**Example 3:**

**Input:** n = 5, x = 1

**Output:** false

**Explanation:**

The number does not contain digit 1. Thus, the answer is `false`.

**Constraints:**

*   <code>0 <= n <= 10<sup>5</sup></code>
*   `0 <= x <= 9`

## Solution

```java
public class Solution {
    public boolean validDigit(int n, int x) {
        boolean value = false;
        while (n > 0) {
            int rem = n % 10;
            n /= 10;
            if (rem == x) {
                value = true;
            }
            if (n == 0 && rem == x) {
                value = false;
            }
        }
        return value;
    }
}
```