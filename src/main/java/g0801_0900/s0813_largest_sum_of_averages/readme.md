## 813\. Largest Sum of Averages

Medium

You are given an integer array `nums` and an integer `k`. You can partition the array into **at most** `k` non-empty adjacent subarrays. The **score** of a partition is the sum of the averages of each subarray.

Note that the partition must use every integer in `nums`, and that the score is not necessarily an integer.

Return _the maximum **score** you can achieve of all the possible partitions_. Answers within <code>10<sup>-6</sup></code> of the actual answer will be accepted.

**Example 1:**

**Input:** nums = [9,1,2,3,9], k = 3

**Output:** 20.00000

**Explanation:** 

The best choice is to partition nums into [9], [1, 2, 3], [9]. The answer is 9 + (1 + 2 + 3) / 3 + 9 = 20. 

We could have also partitioned nums into [9, 1], [2], [3, 9], for example. 

That partition would lead to a score of 5 + 2 + 6 = 13, which is worse.

**Example 2:**

**Input:** nums = [1,2,3,4,5,6,7], k = 4

**Output:** 20.50000

**Constraints:**

*   `1 <= nums.length <= 100`
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>
*   `1 <= k <= nums.length`

## Solution

```java
public class Solution {
    public double largestSumOfAverages(int[] nums, int k) {
        return helper(nums, k, 0, new Double[k + 1][nums.length]);
    }

    private double helper(int[] nums, int k, int idx, Double[][] memo) {
        if (memo[k][idx] != null) {
            return memo[k][idx];
        }
        double maxAvg = 0;
        double sum = 0;
        for (int i = idx; i <= nums.length - k; i++) {
            sum += nums[i];
            if (k - 1 > 0) {
                maxAvg = Math.max(maxAvg, (sum / (i - idx + 1)) + helper(nums, k - 1, i + 1, memo));
            } else if (i == nums.length - 1) {
                maxAvg = Math.max(maxAvg, sum / (i - idx + 1));
            }
        }
        memo[k][idx] = maxAvg;
        return maxAvg;
    }
}
```