[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 600\. Non-negative Integers without Consecutive Ones

Hard

Given a positive integer `n`, return the number of the integers in the range `[0, n]` whose binary representations **do not** contain consecutive ones.

**Example 1:**

**Input:** n = 5

**Output:** 5

**Explanation:**

    Here are the non-negative integers <= 5 with their corresponding binary representations:
    0 : 0
    1 : 1
    2 : 10
    3 : 11
    4 : 100
    5 : 101
    Among them, only integer 3 disobeys the rule (two consecutive ones) and the other 5 satisfy the rule. 

**Example 2:**

**Input:** n = 1

**Output:** 2 

**Example 3:**

**Input:** n = 2

**Output:** 3 

**Constraints:**

*   <code>1 <= n <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int findIntegers(int n) {
        int[] f = new int[32];
        f[0] = 1;
        f[1] = 2;
        int ans = 0;
        int preBit = 0;
        for (int i = 2; i < 32; i++) {
            f[i] = f[i - 1] + f[i - 2];
        }
        for (int k = 31; k >= 0; k--) {
            if ((n & (1 << k)) != 0) {
                // if that bit is on
                ans += f[k];
                if (preBit != 0) {
                    return ans;
                }
                preBit = 1;
            } else {
                preBit = 0;
            }
        }
        return ans + 1;
    }
}
```