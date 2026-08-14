[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3886\. Sum of Sortable Integers

Hard

You are given an integer array `nums` of length `n`.

An integer `k` is called **sortable** if `k` **divides** `n` and you can sort `nums` in **non-decreasing** order by sequentially performing the following operations:

*   Partition `nums` into **consecutive non-empty subarrays** of length `k`.
*   **Cyclically rotate each subarray independently** any number of times to the left or to the right.

Return an integer denoting the sum of all possible sortable integers `k`.

**Example 1:**

**Input:** nums = [3,1,2]

**Output:** 3

**Explanation:**

*   For `n = 3`, possible divisors are 1 and 3.
*   For `k = 1`: each subarray has one element. No rotation can sort the array.
*   For `k = 3`: the single subarray `[3, 1, 2]` can be rotated once to produce `[1, 2, 3]`, which is sorted.
*   Only `k = 3` is sortable. Hence, the answer is 3.

**Example 2:**

**Input:** nums = [7,6,5]

**Output:** 0

**Explanation:**

*   For `n = 3`, possible divisors are 1 and 3.
*   For `k = 1`: each subarray has one element. No rotation can sort the array.
*   For `k = 3`: the single subarray `[7, 6, 5]` cannot be rotated into non-decreasing order.
*   No `k` is sortable. Hence, the answer is 0.

**Example 3:**

**Input:** nums = [5,8]

**Output:** 3

**Explanation:**

*   For `n = 2`, possible divisors are 1 and 2.
*   Since `[5, 8]` is already sorted, every divisor is sortable. Hence, the answer is `1 + 2 = 3`.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    // Changed method name from sumOfSortableIntegers to sortableIntegers
    public int sortableIntegers(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int[] prefMax = new int[n];
        prefMax[0] = nums[0];
        for (int i = 1; i < n; i++) {
            prefMax[i] = Math.max(prefMax[i - 1], nums[i]);
        }
        int[] suffMin = new int[n];
        suffMin[n - 1] = nums[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            suffMin[i] = Math.min(suffMin[i + 1], nums[i]);
        }
        int[] prefDrops = new int[n];
        for (int i = 1; i < n; i++) {
            prefDrops[i] = prefDrops[i - 1] + (nums[i - 1] > nums[i] ? 1 : 0);
        }
        int sumOfK = 0;
        for (int k = 1; k <= n; k++) {
            if (n % k == 0 && (isSortable(k, n, nums, prefMax, suffMin, prefDrops))) {
                sumOfK += k;
            }
        }
        return sumOfK;
    }

    private boolean isSortable(
            int k, int n, int[] nums, int[] prefMax, int[] suffMin, int[] prefDrops) {
        for (int idx = k; idx < n; idx += k) {
            if (prefMax[idx - 1] > suffMin[idx]) {
                return false;
            }
        }
        for (int start = 0; start < n; start += k) {
            int end = start + k - 1;
            int internalDrops = prefDrops[end] - prefDrops[start];
            int cyclicDrop = (nums[end] > nums[start]) ? 1 : 0;
            if (internalDrops + cyclicDrop > 1) {
                return false;
            }
        }
        return true;
    }
}
```