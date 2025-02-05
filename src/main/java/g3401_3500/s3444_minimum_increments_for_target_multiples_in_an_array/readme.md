[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3444\. Minimum Increments for Target Multiples in an Array

Hard

You are given two arrays, `nums` and `target`.

In a single operation, you may increment any element of `nums` by 1.

Return **the minimum number** of operations required so that each element in `target` has **at least** one multiple in `nums`.

**Example 1:**

**Input:** nums = [1,2,3], target = [4]

**Output:** 1

**Explanation:**

The minimum number of operations required to satisfy the condition is 1.

*   Increment 3 to 4 with just one operation, making 4 a multiple of itself.

**Example 2:**

**Input:** nums = [8,4], target = [10,5]

**Output:** 2

**Explanation:**

The minimum number of operations required to satisfy the condition is 2.

*   Increment 8 to 10 with 2 operations, making 10 a multiple of both 5 and 10.

**Example 3:**

**Input:** nums = [7,9,10], target = [7]

**Output:** 0

**Explanation:**

Target 7 already has a multiple in nums, so no additional operations are needed.

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   `1 <= target.length <= 4`
*   `target.length <= nums.length`
*   <code>1 <= nums[i], target[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int minimumIncrements(int[] nums, int[] target) {
        int m = target.length;
        int fullMask = (1 << m) - 1;
        long[] lcmArr = new long[1 << m];
        for (int mask = 1; mask < (1 << m); mask++) {
            long l = 1;
            for (int j = 0; j < m; j++) {
                if ((mask & (1 << j)) != 0) {
                    l = lcm(l, target[j]);
                }
            }
            lcmArr[mask] = l;
        }
        long inf = Long.MAX_VALUE / 2;
        long[] dp = new long[1 << m];
        for (int i = 0; i < (1 << m); i++) {
            dp[i] = inf;
        }
        dp[0] = 0;
        for (int x : nums) {
            long[] newdp = dp.clone();
            for (int mask = 1; mask < (1 << m); mask++) {
                long l = lcmArr[mask];
                long r = x % l;
                long cost = (r == 0 ? 0 : l - r);
                for (int old = 0; old < (1 << m); old++) {
                    int newMask = old | mask;
                    newdp[newMask] = Math.min(newdp[newMask], dp[old] + cost);
                }
            }
            dp = newdp;
        }
        return (int) dp[fullMask];
    }

    private long gcd(long a, long b) {
        return b == 0 ? a : gcd(b, a % b);
    }

    private long lcm(long a, long b) {
        return a / gcd(a, b) * b;
    }
}
```