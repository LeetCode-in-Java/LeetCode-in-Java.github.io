[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1043\. Partition Array for Maximum Sum

Medium

Given an integer array `arr`, partition the array into (contiguous) subarrays of length **at most** `k`. After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return _the largest sum of the given array after partitioning. Test cases are generated so that the answer fits in a **32-bit** integer._

**Example 1:**

**Input:** arr = [1,15,7,9,2,5,10], k = 3

**Output:** 84

**Explanation:** arr becomes [15,15,15,9,10,10,10]

**Example 2:**

**Input:** arr = [1,4,1,5,7,3,6,1,9,9,3], k = 4

**Output:** 83

**Example 3:**

**Input:** arr = [1], k = 1

**Output:** 1

**Constraints:**

*   `1 <= arr.length <= 500`
*   <code>0 <= arr[i] <= 10<sup>9</sup></code>
*   `1 <= k <= arr.length`

## Solution

```java
public class Solution {
    public int maxSumAfterPartitioning(int[] arr, int k) {
        int n = arr.length;
        int[] dp = new int[n];
        for (int right = 0; right < n; right++) {
            int localMax = arr[right];
            for (int left = right; left > Math.max(-1, right - k); left--) {
                localMax = Math.max(localMax, arr[left]);
                if (left == 0) {
                    dp[right] = Math.max(dp[right], (right - left + 1) * localMax);
                } else {
                    dp[right] = Math.max(dp[right], dp[left - 1] + (right - left + 1) * localMax);
                }
            }
        }
        return dp[n - 1];
    }
}
```