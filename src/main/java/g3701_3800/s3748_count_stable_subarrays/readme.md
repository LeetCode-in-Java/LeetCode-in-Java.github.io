[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3748\. Count Stable Subarrays

Hard

You are given an integer array `nums`.

A **non-empty subarrays** of `nums` is called **stable** if it contains **no inversions**, i.e., there is no pair of indices `i < j` such that `nums[i] > nums[j]`.

You are also given a **2D integer array** `queries` of length `q`, where each <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> represents a query. For each query <code>[l<sub>i</sub>, r<sub>i</sub>]</code>, compute the number of **stable subarrays** that lie entirely within the segment <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>.

Return an integer array `ans` of length `q`, where `ans[i]` is the answer to the <code>i<sup>th</sup></code> query.

**Note**:

*   A single element subarray is considered stable.

**Example 1:**

**Input:** nums = [3,1,2], queries = \[\[0,1],[1,2],[0,2]]

**Output:** [2,3,4]

**Explanation:**

*   For `queries[0] = [0, 1]`, the subarray is `[nums[0], nums[1]] = [3, 1]`.
    *   The stable subarrays are `[3]` and `[1]`. The total number of stable subarrays is 2.
*   For `queries[1] = [1, 2]`, the subarray is `[nums[1], nums[2]] = [1, 2]`.
    *   The stable subarrays are `[1]`, `[2]`, and `[1, 2]`. The total number of stable subarrays is 3.
*   For `queries[2] = [0, 2]`, the subarray is `[nums[0], nums[1], nums[2]] = [3, 1, 2]`.
    *   The stable subarrays are `[3]`, `[1]`, `[2]`, and `[1, 2]`. The total number of stable subarrays is 4.

Thus, `ans = [2, 3, 4]`.

**Example 2:**

**Input:** nums = [2,2], queries = \[\[0,1],[0,0]]

**Output:** [3,1]

**Explanation:**

*   For `queries[0] = [0, 1]`, the subarray is `[nums[0], nums[1]] = [2, 2]`.
    *   The stable subarrays are `[2]`, `[2]`, and `[2, 2]`. The total number of stable subarrays is 3.
*   For `queries[1] = [0, 0]`, the subarray is `[nums[0]] = [2]`.
    *   The stable subarray is `[2]`. The total number of stable subarrays is 1.

Thus, `ans = [3, 1]`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>
*   <code>0 <= l<sub>i</sub> <= r<sub>i</sub> <= nums.length - 1</code>

## Solution

```java
public class Solution {
    public long[] countStableSubarrays(int[] nums, int[][] queries) {
        int n = nums.length;
        long[] preSum = new long[n + 1];
        int[] idx = new int[n];
        int[] end = new int[n];
        int cnt = 0;
        int prv = -1;
        for (int i = 0; i < n; i++) {
            if (nums[i] >= prv) {
                cnt++;
            } else {
                cnt = 1;
            }
            prv = nums[i];
            preSum[i + 1] = preSum[i] + cnt;
            idx[i] = cnt - 1;
        }
        end[n - 1] = n - 1;
        for (int i = n - 2; i >= 0; i--) {
            if (idx[i] + 1 == idx[i + 1]) {
                end[i] = end[i + 1];
            } else {
                end[i] = i;
            }
        }
        long[] ans = new long[queries.length];
        for (int l = 0; l < queries.length; l++) {
            int i = queries[l][0];
            int j = queries[l][1];
            long res = preSum[j + 1] - preSum[i];
            int endIdx = Math.min(end[i], j);
            res -= (long) (endIdx - i + 1) * idx[i];
            ans[l] = res;
        }
        return ans;
    }
}
```