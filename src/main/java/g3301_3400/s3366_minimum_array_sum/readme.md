[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3366\. Minimum Array Sum

Medium

You are given an integer array `nums` and three integers `k`, `op1`, and `op2`.

You can perform the following operations on `nums`:

*   **Operation 1**: Choose an index `i` and divide `nums[i]` by 2, **rounding up** to the nearest whole number. You can perform this operation at most `op1` times, and not more than **once** per index.
*   **Operation 2**: Choose an index `i` and subtract `k` from `nums[i]`, but only if `nums[i]` is greater than or equal to `k`. You can perform this operation at most `op2` times, and not more than **once** per index.

**Note:** Both operations can be applied to the same index, but at most once each.

Return the **minimum** possible **sum** of all elements in `nums` after performing any number of operations.

**Example 1:**

**Input:** nums = [2,8,3,19,3], k = 3, op1 = 1, op2 = 1

**Output:** 23

**Explanation:**

*   Apply Operation 2 to `nums[1] = 8`, making `nums[1] = 5`.
*   Apply Operation 1 to `nums[3] = 19`, making `nums[3] = 10`.
*   The resulting array becomes `[2, 5, 3, 10, 3]`, which has the minimum possible sum of 23 after applying the operations.

**Example 2:**

**Input:** nums = [2,4,3], k = 3, op1 = 2, op2 = 1

**Output:** 3

**Explanation:**

*   Apply Operation 1 to `nums[0] = 2`, making `nums[0] = 1`.
*   Apply Operation 1 to `nums[1] = 4`, making `nums[1] = 2`.
*   Apply Operation 2 to `nums[2] = 3`, making `nums[2] = 0`.
*   The resulting array becomes `[1, 2, 0]`, which has the minimum possible sum of 3 after applying the operations.

**Constraints:**

*   `1 <= nums.length <= 100`
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>
*   <code>0 <= k <= 10<sup>5</sup></code>
*   `0 <= op1, op2 <= nums.length`

## Solution

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int minArraySum(int[] nums, int k, int op1, int op2) {
        Arrays.sort(nums);
        int high = lowerBound(nums, k * 2 - 1);
        int low = lowerBound(nums, k);
        int n = nums.length;
        for (int i = n - 1; i >= high; i--) {
            if (op1 > 0) {
                nums[i] = (nums[i] + 1) / 2;
                op1--;
            }
            if (op2 > 0) {
                nums[i] -= k;
                op2--;
            }
        }
        Map<Integer, Integer> count = new HashMap<>();
        int odd = 0;
        for (int i = low; i < high; i++) {
            if (op2 > 0) {
                nums[i] -= k;
                if (k % 2 > 0 && nums[i] % 2 > 0) {
                    count.merge(nums[i], 1, Integer::sum);
                }
                op2--;
            } else {
                odd += nums[i] % 2;
            }
        }
        Arrays.sort(nums, 0, high);
        int ans = 0;
        if (k % 2 > 0) {
            for (int i = high - op1; i < high && odd > 0; i++) {
                int x = nums[i];
                if (count.containsKey(x)) {
                    if (count.merge(x, -1, Integer::sum) == 0) {
                        count.remove(x);
                    }
                    odd--;
                    ans--;
                }
            }
        }
        for (int i = high - 1; i >= 0 && op1 > 0; i--, op1--) {
            nums[i] = (nums[i] + 1) / 2;
        }
        for (int x : nums) {
            ans += x;
        }
        return ans;
    }

    private int lowerBound(int[] nums, int target) {
        int left = -1;
        int right = nums.length;
        while (left + 1 < right) {
            int mid = (left + right) >>> 1;
            if (nums[mid] >= target) {
                right = mid;
            } else {
                left = mid;
            }
        }
        return right;
    }
}
```