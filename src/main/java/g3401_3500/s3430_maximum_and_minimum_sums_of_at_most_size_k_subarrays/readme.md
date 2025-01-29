[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3430\. Maximum and Minimum Sums of at Most Size K Subarrays

Hard

You are given an integer array `nums` and a **positive** integer `k`. Return the sum of the **maximum** and **minimum** elements of all **non-empty subarrays** with **at most** `k` elements.

**Example 1:**

**Input:** nums = [1,2,3], k = 2

**Output:** 20

**Explanation:**

The subarrays of `nums` with at most 2 elements are:

| **Subarray** | Minimum | Maximum | Sum |
|--------------|---------|---------|-----|
| `[1]`        | 1       | 1       | 2   |
| `[2]`        | 2       | 2       | 4   |
| `[3]`        | 3       | 3       | 6   |
| `[1, 2]`     | 1       | 2       | 3   |
| `[2, 3]`     | 2       | 3       | 5   |
| **Final Total** |         |         | 20  |

The output would be 20.

**Example 2:**

**Input:** nums = [1,-3,1], k = 2

**Output:** \-6

**Explanation:**

The subarrays of `nums` with at most 2 elements are:

| **Subarray**   | Minimum | Maximum | Sum  |
|----------------|---------|---------|------|
| `[1]`          | 1       | 1       | 2    |
| `[-3]`         | -3      | -3      | -6   |
| `[1]`          | 1       | 1       | 2    |
| `[1, -3]`      | -3      | 1       | -2   |
| `[-3, 1]`      | -3      | 1       | -2   |
| **Final Total**|         |         | -6   |

The output would be -6.

**Constraints:**

*   `1 <= nums.length <= 80000`
*   `1 <= k <= nums.length`
*   <code>-10<sup>6</sup> <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public long minMaxSubarraySum(int[] nums, int k) {
        long sum = solve(nums, k);
        for (int i = 0; i < nums.length; i++) {
            nums[i] = -nums[i];
        }
        return sum - solve(nums, k);
    }

    private long solve(int[] nums, int k) {
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        int[] st = new int[n];
        int top = -1;
        for (int i = 0; i < n; i++) {
            int num = nums[i];
            while (top != -1 && num < nums[st[top]]) {
                right[st[top--]] = i;
            }
            left[i] = top == -1 ? -1 : st[top];
            st[++top] = i;
        }
        while (0 <= top) {
            right[st[top--]] = n;
        }
        long ans = 0;
        for (int i = 0; i < n; i++) {
            int num = nums[i];
            int l = Math.min(i - left[i], k);
            int r = Math.min(right[i] - i, k);
            if (l + r - 1 <= k) {
                ans += (long) num * l * r;
            } else {
                long cnt = (long) (k - r + 1) * r + (long) (l + r - k - 1) * (r + k - l) / 2;
                ans += num * cnt;
            }
        }
        return ans;
    }
}
```