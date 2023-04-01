[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1799\. Maximize Score After N Operations

Hard

You are given `nums`, an array of positive integers of size `2 * n`. You must perform `n` operations on this array.

In the <code>i<sup>th</sup></code> operation **(1-indexed)**, you will:

*   Choose two elements, `x` and `y`.
*   Receive a score of `i * gcd(x, y)`.
*   Remove `x` and `y` from `nums`.

Return _the maximum score you can receive after performing_ `n` _operations._

The function `gcd(x, y)` is the greatest common divisor of `x` and `y`.

**Example 1:**

**Input:** nums = [1,2]

**Output:** 1

**Explanation:** The optimal choice of operations is:

v(1 \* gcd(1, 2)) = 1

**Example 2:**

**Input:** nums = [3,4,6,8]

**Output:** 11

**Explanation:** The optimal choice of operations is:

(1 \* gcd(3, 6)) + (2 \* gcd(4, 8)) = 3 + 8 = 11

**Example 3:**

**Input:** nums = [1,2,3,4,5,6]

**Output:** 14

**Explanation:** The optimal choice of operations is:

(1 \* gcd(1, 5)) + (2 \* gcd(2, 4)) + (3 \* gcd(3, 6)) = 1 + 4 + 9 = 14

**Constraints:**

*   `1 <= n <= 7`
*   `nums.length == 2 * n`
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public int maxScore(int[] nums) {
        int n = nums.length;
        Integer[] memo = new Integer[1 << n];
        return helper(1, 0, nums, memo);
    }

    private int helper(int operationNumber, int mask, int[] nums, Integer[] memo) {
        int n = nums.length;
        if (memo[mask] != null) {
            return memo[mask];
        }
        if (operationNumber > n / 2) {
            return 0;
        }
        int maxScore = Integer.MIN_VALUE;
        for (int i = 0; i < n; i++) {
            if ((mask & (1 << i)) == 0) {
                for (int j = i + 1; j < n; j++) {
                    if ((mask & (1 << j)) == 0) {
                        int score = operationNumber * gcd(nums[i], nums[j]);
                        int score2 =
                                helper(operationNumber + 1, mask | (1 << i) | (1 << j), nums, memo);
                        maxScore = Math.max(maxScore, score + score2);
                    }
                }
            }
        }
        memo[mask] = maxScore;
        return maxScore;
    }

    private int gcd(int a, int b) {
        if (b == 0) {
            return a;
        }
        return gcd(b, a % b);
    }
}
```