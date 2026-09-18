[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3911\. K-th Smallest Remaining Even Integer in Subarray Queries

Hard

You are given an integer array `nums` where `nums` is **strictly increasing**.

You are also given a 2D integer array `queries`, where <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, k<sub>i</sub>]</code>.

For each query <code>[l<sub>i</sub>, r<sub>i</sub>, k<sub>i</sub>]</code>:

*   Consider the **non-empty subarrays** <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>
*   From the **infinite** sequence of all **positive even integers**: `2, 4, 6, 8, 10, 12, 14, ...`
*   **Remove** all elements that appear in the **subarray** <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>.
*   Find the <code>k<sub>i</sub><sup>th</sup></code> **smallest integer** remaining in the sequence after the removals.

Return an integer array `ans`, where `ans[i]` is the result for the <code>i<sup>th</sup></code> query.

**Example 1:**

**Input:** nums = [1,4,7], queries = \[\[0,2,1],[1,1,2],[0,0,3]]

**Output:** [2,6,6]

**Explanation:**

| `i` | `queries[i]` | `nums[l_i..r_i]` | Removed Evens | Remaining Evens | `k_i` | `ans[i]` |
|---:|---|---|---|---|---:|---:|
| 0 | `[0, 2, 1]` | `[1, 4, 7]` | `[4]` | 2, 6, 8, ... | 1 | 2 |
| 1 | `[1, 1, 2]` | `[4]` | `[4]` | 2, 6, 8, ... | 2 | 6 |
| 2 | `[0, 0, 3]` | `[1]` | `[]` | 2, 4, 6, ... | 3 | 6 |

Thus, `ans = [2, 6, 6]`.

**Example 2:**

**Input:** nums = [2,5,8], queries = \[\[0,1,2],[1,2,1],[0,2,4]]

**Output:** [6,2,12]

**Explanation:**

| `i` | `queries[i]` | `nums[l_i..r_i]` | Removed Evens | Remaining Evens | `k_i` | `ans[i]` |
|---:|---|---|---|---|---:|---:|
| 0 | `[0, 1, 2]` | `[2, 5]` | `[2]` | 4, 6, 8, ... | 2 | 6 |
| 1 | `[1, 2, 1]` | `[5, 8]` | `[8]` | 2, 4, 6, ... | 1 | 2 |
| 2 | `[0, 2, 4]` | `[2, 5, 8]` | `[2, 8]` | 4, 6, 10, 12, ... | 4 | 12 |

Thus, `ans = [6, 2, 12]`.

**Example 3:**

**Input:** nums = [3,6], queries = \[\[0,1,1],[1,1,3]]

**Output:** [2,8]

**Explanation:**

| `i` | `queries[i]` | `nums[l_i..r_i]` | Removed Evens | Remaining Evens | `k_i` | `ans[i]` |
|---:|---|---|---|---|---:|---:|
| 0 | `[0, 1, 1]` | `[3, 6]` | `[6]` | 2, 4, 8, ... | 1 | 2 |
| 1 | `[1, 1, 3]` | `[6]` | `[6]` | 2, 4, 8, ... | 3 | 8 |

Thus, `ans = [2, 8]`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `nums` is strictly increasing
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, k<sub>i</sub>]</code>
*   <code>0 <= l<sub>i</sub> <= r<sub>i</sub> < nums.length</code>
*   <code>1 <= k<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int[] kthRemainingInteger(int[] nums, int[][] queries) {
        int n = nums.length;
        int[] ans = new int[queries.length];
        int[] prefix = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            prefix[i] = prefix[i - 1] + ((nums[i - 1] % 2 == 0) ? 1 : 0);
        }
        for (int q = 0; q < queries.length; q++) {
            int l = queries[q][0];
            int r = queries[q][1];
            int k = queries[q][2];
            int lowerCnt = (nums[l] - 1) / 2;
            int upperCnt = nums[r] / 2;
            int remove = prefix[r + 1] - prefix[l];
            // first segment
            if (lowerCnt >= k) {
                ans[q] = 2 * k;
                continue;
            }
            // third segment
            if (upperCnt - remove < k) {
                ans[q] = 2 * (k + remove);
            } else {
                // middle segment
                int s = l;
                int e = r;
                while (s <= e) {
                    int m = s + (e - s) / 2;
                    int u = nums[m] / 2;
                    int rem = prefix[m + 1] - prefix[l];
                    if (u - rem < k) {
                        s = m + 1;
                    } else {
                        e = m - 1;
                    }
                }
                ans[q] = 2 * (k + (prefix[s] - prefix[l]));
            }
        }
        return ans;
    }
}
```