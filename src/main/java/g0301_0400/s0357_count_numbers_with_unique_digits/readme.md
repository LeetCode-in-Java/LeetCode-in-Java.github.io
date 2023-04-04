[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 357\. Count Numbers with Unique Digits

Medium

Given an integer `n`, return the count of all numbers with unique digits, `x`, where <code>0 <= x < 10<sup>n</sup></code>.

**Example 1:**

**Input:** n = 2

**Output:** 91

**Explanation:** The answer should be the total numbers in the range of 0 ≤ x < 100, excluding 11,22,33,44,55,66,77,88,99

**Example 2:**

**Input:** n = 0

**Output:** 1

**Constraints:**

*   `0 <= n <= 8`

## Solution

```java
public class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        int ans = 1;
        for (int i = 1; i <= n; i++) {
            int mul = 1;
            for (int j = 1; j < i; j++) {
                mul *= (10 - j);
            }
            ans = ans + 9 * mul;
        }
        return ans;
    }
}
```