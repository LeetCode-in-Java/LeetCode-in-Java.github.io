[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3469\. Find Minimum Cost to Remove Array Elements

Medium

You are given an integer array `nums`. Your task is to remove **all elements** from the array by performing one of the following operations at each step until `nums` is empty:

*   Choose any two elements from the first three elements of `nums` and remove them. The cost of this operation is the **maximum** of the two elements removed.
*   If fewer than three elements remain in `nums`, remove all the remaining elements in a single operation. The cost of this operation is the **maximum** of the remaining elements.

Return the **minimum** cost required to remove all the elements.

**Example 1:**

**Input:** nums = [6,2,8,4]

**Output:** 12

**Explanation:**

Initially, `nums = [6, 2, 8, 4]`.

*   In the first operation, remove `nums[0] = 6` and `nums[2] = 8` with a cost of `max(6, 8) = 8`. Now, `nums = [2, 4]`.
*   In the second operation, remove the remaining elements with a cost of `max(2, 4) = 4`.

The cost to remove all elements is `8 + 4 = 12`. This is the minimum cost to remove all elements in `nums`. Hence, the output is 12.

**Example 2:**

**Input:** nums = [2,1,3,3]

**Output:** 5

**Explanation:**

Initially, `nums = [2, 1, 3, 3]`.

*   In the first operation, remove `nums[0] = 2` and `nums[1] = 1` with a cost of `max(2, 1) = 2`. Now, `nums = [3, 3]`.
*   In the second operation remove the remaining elements with a cost of `max(3, 3) = 3`.

The cost to remove all elements is `2 + 3 = 5`. This is the minimum cost to remove all elements in `nums`. Hence, the output is 5.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static final int INF = (int) 1e9;

    public int minCost(int[] nums) {
        int n = nums.length;
        if (n % 2 == 0) {
            nums = Arrays.copyOf(nums, ++n);
        }
        int[] dp = new int[n];
        for (int j = 1; j < n - 1; j += 2) {
            int cost1 = INF;
            int cost2 = INF;
            int max = Math.max(nums[j], nums[j + 1]);
            for (int i = 0; i < j; ++i) {
                cost1 = Math.min(cost1, dp[i] + Math.max(nums[i], nums[j + 1]));
                cost2 = Math.min(cost2, dp[i] + Math.max(nums[i], nums[j]));
                dp[i] += max;
            }
            dp[j] = cost1;
            dp[j + 1] = cost2;
        }
        int result = INF;
        for (int i = 0; i < n; ++i) {
            result = Math.min(result, dp[i] + nums[i]);
        }
        return result;
    }
}
```