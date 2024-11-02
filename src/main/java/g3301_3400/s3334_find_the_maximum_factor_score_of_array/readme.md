[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3334\. Find the Maximum Factor Score of Array

Medium

You are given an integer array `nums`.

The **factor score** of an array is defined as the _product_ of the LCM and GCD of all elements of that array.

Return the **maximum factor score** of `nums` after removing **at most** one element from it.

**Note** that _both_ the LCM and GCD of a single number are the number itself, and the _factor score_ of an **empty** array is 0.

The term `lcm(a, b)` denotes the **least common multiple** of `a` and `b`.

The term `gcd(a, b)` denotes the **greatest common divisor** of `a` and `b`.

**Example 1:**

**Input:** nums = [2,4,8,16]

**Output:** 64

**Explanation:**

On removing 2, the GCD of the rest of the elements is 4 while the LCM is 16, which gives a maximum factor score of `4 * 16 = 64`.

**Example 2:**

**Input:** nums = [1,2,3,4,5]

**Output:** 60

**Explanation:**

The maximum factor score of 60 can be obtained without removing any elements.

**Example 3:**

**Input:** nums = [3]

**Output:** 9

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 30`

## Solution

```java
public class Solution {
    public long maxScore(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return (long) nums[0] * nums[0];
        }
        long[][] lToR = new long[n][2];
        long[][] rToL = new long[n][2];
        for (int i = 0; i < n; i++) {
            if (i == 0) {
                lToR[i][0] = lToR[i][1] = nums[i];
                rToL[n - i - 1][0] = rToL[n - i - 1][1] = nums[n - i - 1];
            } else {
                rToL[n - i - 1][0] = gcd(nums[n - i - 1], rToL[n - i][0]);
                lToR[i][0] = gcd(nums[i], lToR[i - 1][0]);

                rToL[n - i - 1][1] = lcm(nums[n - i - 1], rToL[n - i][1]);
                lToR[i][1] = lcm(nums[i], lToR[i - 1][1]);
            }
        }
        long max = 0;
        for (int i = 0; i < n; i++) {
            long gcd = i == 0 ? rToL[i + 1][0] : getLong(i, n, lToR, rToL);
            max = Math.max(max, gcd * (i == 0 ? rToL[i + 1][1] : getaLong(i, n, lToR, rToL)));
        }
        return Math.max(max, rToL[0][0] * rToL[0][1]);
    }

    private long getaLong(int i, int n, long[][] lToR, long[][] rToL) {
        return i == n - 1 ? lToR[i - 1][1] : lcm(rToL[i + 1][1], lToR[i - 1][1]);
    }

    private long getLong(int i, int n, long[][] lToR, long[][] rToL) {
        return i == n - 1 ? lToR[i - 1][0] : gcd(rToL[i + 1][0], lToR[i - 1][0]);
    }

    private long gcd(long a, long b) {
        if (b == 0) {
            return a;
        }
        return gcd(b, a % b);
    }

    private long lcm(long a, long b) {
        return a * b / gcd(a, b);
    }
}
```