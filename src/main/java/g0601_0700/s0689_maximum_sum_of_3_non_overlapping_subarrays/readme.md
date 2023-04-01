[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 689\. Maximum Sum of 3 Non-Overlapping Subarrays

Hard

Given an integer array `nums` and an integer `k`, find three non-overlapping subarrays of length `k` with maximum sum and return them.

Return the result as a list of indices representing the starting position of each interval (**0-indexed**). If there are multiple answers, return the lexicographically smallest one.

**Example 1:**

**Input:** nums = [1,2,1,2,6,7,5,1], k = 2

**Output:** [0,3,5]

**Explanation:**

Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].

We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger. 

**Example 2:**

**Input:** nums = [1,2,1,2,1,2,1,2,1], k = 2

**Output:** [0,2,4] 

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] < 2<sup>16</sup></code>
*   `1 <= k <= floor(nums.length / 3)`

## Solution

```java
public class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        int len = nums.length;
        if (len < 3 * k) {
            return new int[] {};
        }
        int[] res = new int[3];
        int[][] left = new int[2][len];
        int[][] right = new int[2][len];
        int s = 0;
        for (int i = 0; i < k; i++) {
            s += nums[i];
        }
        left[0][k - 1] = s;
        for (int i = k; i + 2 * k <= len; i++) {
            s = s + nums[i] - nums[i - k];
            if (s > left[0][i - 1]) {
                left[0][i] = s;
                left[1][i] = i - k + 1;
            } else {
                left[0][i] = left[0][i - 1];
                left[1][i] = left[1][i - 1];
            }
        }
        s = 0;
        for (int i = len - 1; i >= len - k; i--) {
            s += nums[i];
        }
        right[0][len - k] = s;
        right[1][len - k] = len - k;
        for (int i = len - k - 1; i >= 0; i--) {
            s = s + nums[i] - nums[i + k];
            if (s >= right[0][i + 1]) {
                right[0][i] = s;
                right[1][i] = i;
            } else {
                right[0][i] = right[0][i + 1];
                right[1][i] = right[1][i + 1];
            }
        }
        int mid = 0;
        for (int i = k; i < 2 * k; i++) {
            mid += nums[i];
        }
        int max = 0;
        for (int i = k; i + 2 * k <= len; i++) {
            int total = left[0][i - 1] + right[0][i + k] + mid;
            if (total > max) {
                res[0] = left[1][i - 1];
                res[1] = i;
                res[2] = right[1][i + k];
                max = total;
            }
            mid = mid + nums[i + k] - nums[i];
        }
        return res;
    }
}
```