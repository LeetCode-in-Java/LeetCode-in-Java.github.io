## 1425\. Constrained Subsequence Sum

Hard

Given an integer array `nums` and an integer `k`, return the maximum sum of a **non-empty** subsequence of that array such that for every two **consecutive** integers in the subsequence, `nums[i]` and `nums[j]`, where `i < j`, the condition `j - i <= k` is satisfied.

A _subsequence_ of an array is obtained by deleting some number of elements (can be zero) from the array, leaving the remaining elements in their original order.

**Example 1:**

**Input:** nums = [10,2,-10,5,20], k = 2

**Output:** 37

**Explanation:** The subsequence is [10, 2, 5, 20].

**Example 2:**

**Input:** nums = [-1,-2,-3], k = 1

**Output:** -1

**Explanation:** The subsequence must be non-empty, so we choose the largest number.

**Example 3:**

**Input:** nums = [10,-2,-10,-5,20], k = 2

**Output:** 23

**Explanation:** The subsequence is [10, -2, -5, 20].

**Constraints:**

*   <code>1 <= k <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.LinkedList;

public class Solution {
    public int constrainedSubsetSum(int[] nums, int k) {
        int n = nums.length;
        int res = Integer.MIN_VALUE;
        LinkedList<int[]> mono = new LinkedList<>();

        for (int i = 0; i < n; i++) {
            int take = nums[i];

            while (!mono.isEmpty() && i - mono.getFirst()[0] > k) {
                mono.removeFirst();
            }
            if (!mono.isEmpty()) {
                int mx = Math.max(0, mono.getFirst()[1]);
                take += mx;
            }
            while (!mono.isEmpty() && take > mono.getLast()[1]) {
                mono.removeLast();
            }

            mono.add(new int[] {i, take});
            res = Math.max(res, take);
        }
        return res;
    }
}
```