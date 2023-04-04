[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 233\. Number of Digit One

Hard

Given an integer `n`, count _the total number of digit_ `1` _appearing in all non-negative integers less than or equal to_ `n`.

**Example 1:**

**Input:** n = 13

**Output:** 6 

**Example 2:**

**Input:** n = 0

**Output:** 0 

**Constraints:**

*   <code>0 <= n <= 10<sup>9</sup></code>

## Solution

```java
@SuppressWarnings("java:S127")
public class Solution {
    public int countDigitOne(int n) {
        int ans = 0;
        // count total number of 1s appearing in every digit, starting from the last digit
        for (int k = n, cum = 0, curr10 = 1; k > 0; curr10 *= 10) {
            int rem = k % 10;
            int q = k / 10;
            if (rem == 0) {
                ans += q * curr10;
            } else if (rem == 1) {
                ans += q * curr10 + cum + 1;
            } else {
                ans += (q + 1) * curr10;
            }
            k = q;
            // if loop is at 3rd last digit and n = 54321, cum is now = 321
            cum += rem * curr10;
        }
        return ans;
    }
}
```