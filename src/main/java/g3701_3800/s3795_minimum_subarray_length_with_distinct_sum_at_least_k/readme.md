[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3795\. Minimum Subarray Length With Distinct Sum At Least K

Medium

You are given an integer array `nums` and an integer `k`.

Return the **minimum** length of a **non-empty subarrays** whose sum of the **distinct** values present in that subarray (each value counted once) is **at least** `k`. If no such subarray exists, return -1.

**Example 1:**

**Input:** nums = [2,2,3,1], k = 4

**Output:** 2

**Explanation:**

The subarray `[2, 3]` has distinct elements `{2, 3}` whose sum is `2 + 3 = 5`, which is at least `k = 4`. Thus, the answer is 2.

**Example 2:**

**Input:** nums = [3,2,3,4], k = 5

**Output:** 2

**Explanation:**

The subarray `[3, 2]` has distinct elements `{3, 2}` whose sum is `3 + 2 = 5`, which is at least `k = 5`. Thus, the answer is 2.

**Example 3:**

**Input:** nums = [5,5,4], k = 5

**Output:** 1

**Explanation:**

The subarray `[5]` has distinct elements `{5}` whose sum is `5`, which is at least `k = 5`. Thus, the answer is 1.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int minLength(int[] nums, int k) {
        for (int i : nums) {
            if (i >= k) {
                return 1;
            }
        }
        int ans = Integer.MAX_VALUE;
        int alt = -1;
        int i = 0;
        int j = 1;
        int sum = nums[0];
        if (sum >= k) {
            return 1;
        }
        Map<Integer, Integer> hm = new HashMap<>();
        hm.put(nums[0], 1);
        while (i < nums.length && j < nums.length) {
            hm.put(nums[j], hm.getOrDefault(nums[j], 0) + 1);
            if (hm.get(nums[j]) == 1) {
                sum += nums[j];
            }
            while (sum >= k) {
                ans = Math.min(ans, j - i + 1);
                hm.put(nums[i], hm.get(nums[i]) - 1);
                if (hm.get(nums[i]) == 0) {
                    sum -= nums[i];
                }
                i++;
            }
            j++;
        }
        return (ans != Integer.MAX_VALUE) ? ans : alt;
    }
}
```