[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3891\. Minimum Increase to Maximize Special Indices

Medium

You are given an integer array `nums` of length `n`.

An index `i` (`0 < i < n - 1`) is **special** if `nums[i] > nums[i - 1]` and `nums[i] > nums[i + 1]`.

You may perform operations where you choose **any** index `i` and **increase** `nums[i]` by 1.

Your goal is to:

*   **Maximize** the number of **special** indices.
*   **Minimize** the total number of **operations** required to achieve that **maximum**.

Return an integer denoting the **minimum** total number of operations required.

**Example 1:**

**Input:** nums = [1,2,2]

**Output:** 1

**Explanation:**

*   Start with `nums = [1, 2, 2]`.
*   Increase `nums[1]` by 1, array becomes `[1, 3, 2]`.
*   The final array is `[1, 3, 2]` has 1 special index, which is the maximum achievable.
*   It is impossible to achieve this number of special indices with fewer operations. Thus, the answer is 1.

**Example 2:**

**Input:** nums = [2,1,1,3]

**Output:** 2

**Explanation:**

*   Start with `nums = [2, 1, 1, 3]`.
*   Perform 2 operations at index 1, array becomes `[2, 3, 1, 3]`.
*   The final array is `[2, 3, 1, 3]` has 1 special index, which is the maximum achievable. Thus, the answer is 2.

**Example 3:**

**Input:** nums = [5,2,1,4,3]

**Output:** 4

**Explanation:**

*   Start with `nums = [5, 2, 1, 4, 3]`.
*   Perform 4 operations at index 1, array becomes `[5, 6, 1, 4, 3]`.
*   The final array is `[5, 6, 1, 4, 3]` has 2 special indices, which is the maximum achievable. Thus, the answer is 4.

**Constraints:**

*   <code>3 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public long minIncrease(int[] nums) {
        int n = nums.length;
        long min = 0;
        if ((n & 1) == 1) {
            for (int i = 1; i < n; i += 2) {
                min += Math.max(Math.max(nums[i - 1], nums[i + 1]) + 1 - nums[i], 0);
            }
        } else {
            long[] starting = new long[] {0, 0};
            for (int i = 1; i < n - 1; i += 2) {
                int firstOp = Math.max(Math.max(nums[i - 1], nums[i + 1]) + 1 - nums[i], 0);
                int secondOp = Math.max(Math.max(nums[i], nums[i + 2]) + 1 - nums[i + 1], 0);
                starting[1] = Math.min(starting[0], starting[1]) + secondOp;
                starting[0] += firstOp;
            }
            min = Math.min(starting[0], starting[1]);
        }
        return min;
    }
}
```