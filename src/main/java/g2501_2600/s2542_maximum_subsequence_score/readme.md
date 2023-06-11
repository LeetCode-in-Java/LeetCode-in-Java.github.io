[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2542\. Maximum Subsequence Score

Medium

You are given two **0-indexed** integer arrays `nums1` and `nums2` of equal length `n` and a positive integer `k`. You must choose a **subsequence** of indices from `nums1` of length `k`.

For chosen indices <code>i<sub>0</sub></code>, <code>i<sub>1</sub></code>, ..., <code>i<sub>k - 1</sub></code>, your **score** is defined as:

*   The sum of the selected elements from `nums1` multiplied with the **minimum** of the selected elements from `nums2`.
*   It can defined simply as: <code>(nums1[i<sub>0</sub>] + nums1[i<sub>1</sub>] +...+ nums1[i<sub>k - 1</sub>]) * min(nums2[i<sub>0</sub>] , nums2[i<sub>1</sub>], ... ,nums2[i<sub>k - 1</sub>])</code>.

Return _the **maximum** possible score._

A **subsequence** of indices of an array is a set that can be derived from the set `{0, 1, ..., n-1}` by deleting some or no elements.

**Example 1:**

**Input:** nums1 = [1,3,3,2], nums2 = [2,1,3,4], k = 3

**Output:** 12

**Explanation:**

The four possible subsequence scores are:

- We choose the indices 0, 1, and 2 with score = (1+3+3) \* min(2,1,3) = 7.

- We choose the indices 0, 1, and 3 with score = (1+3+2) \* min(2,1,4) = 6.

- We choose the indices 0, 2, and 3 with score = (1+3+2) \* min(2,3,4) = 12.

- We choose the indices 1, 2, and 3 with score = (3+3+2) \* min(1,3,4) = 8.

Therefore, we return the max score, which is 12. 

**Example 2:**

**Input:** nums1 = [4,2,3,1,1], nums2 = [7,5,10,9,6], k = 1

**Output:** 30

**Explanation:** Choosing index 2 is optimal: nums1[2] \* nums2[2] = 3 \* 10 = 30 is the maximum possible score. 

**Constraints:**

*   `n == nums1.length == nums2.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>0 <= nums1[i], nums2[j] <= 10<sup>5</sup></code>
*   `1 <= k <= n`

## Solution

```java
import java.util.Arrays;
import java.util.PriorityQueue;

public class Solution {
    private static class PairInfo {
        int val1;
        int val2;

        public PairInfo(int val1, int val2) {
            this.val1 = val1;
            this.val2 = val2;
        }
    }

    public long maxScore(int[] nums1, int[] nums2, int k) {
        int n = nums1.length;
        PairInfo[] nums = new PairInfo[n];
        for (int i = 0; i < n; ++i) {
            nums[i] = new PairInfo(nums1[i], nums2[i]);
        }
        Arrays.sort(
                nums,
                (a, b) -> {
                    if (a.val2 == b.val2) {
                        return a.val1 - b.val1;
                    }
                    return a.val2 - b.val2;
                });
        long sum = 0;
        long ans = 0;
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int i = n - 1; i >= 0; --i) {
            int minVal = nums[i].val2;
            while (pq.size() > k - 1) {
                sum -= pq.poll();
            }
            sum += nums[i].val1;
            pq.add(nums[i].val1);
            if (pq.size() == k) {
                ans = Math.max(ans, sum * minVal);
            }
        }
        return ans;
    }
}
```