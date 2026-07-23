[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3824\. Minimum K to Reduce Array Within Limit

Medium

You are given a **positive** integer array `nums`.

For a positive integer `k`, define `nonPositive(nums, k)` as the **minimum** number of **operations** needed to make every element of `nums` **non-positive**. In one operation, you can choose an index `i` and reduce `nums[i]` by `k`.

Return an integer denoting the **minimum** value of `k` such that <code>nonPositive(nums, k) <= k<sup>2</sup></code>.

**Example 1:**

**Input:** nums = [3,7,5]

**Output:** 3

**Explanation:**

When `k = 3`, <code>nonPositive(nums, k) = 6 <= k<sup>2</sup></code>.

*   Reduce `nums[0] = 3` one time. `nums[0]` becomes `3 - 3 = 0`.
*   Reduce `nums[1] = 7` three times. `nums[1]` becomes `7 - 3 - 3 - 3 = -2`.
*   Reduce `nums[2] = 5` two times. `nums[2]` becomes `5 - 3 - 3 = -1`.

**Example 2:**

**Input:** nums = [1]

**Output:** 1

**Explanation:**

When `k = 1`, <code>nonPositive(nums, k) = 1 <= k<sup>2</sup></code>.

*   Reduce `nums[0] = 1` one time. `nums[0]` becomes `1 - 1 = 0`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int minimumK(int[] nums) {
        int n = nums.length;
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }
        long total = 0;
        for (int x : nums) {
            total += x;
        }
        int left = Math.max((int) Math.ceil(Math.sqrt(n)), (int) Math.ceil(Math.cbrt(total))) - 1;
        int right = (int) Math.ceil(Math.sqrt(nonPositive(nums, left + 1)));
        while (left + 1 < right) {
            int k = (left + right) / 2;
            if (nonPositive(nums, k) <= (long) k * k) {
                right = k;
            } else {
                left = k;
            }
        }
        return left + 1;
    }

    private long nonPositive(int[] nums, int k) {
        long sum = nums.length;
        for (int x : nums) {
            sum += (x - 1) / k;
        }
        return sum;
    }
}
```