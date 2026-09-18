[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3915\. Maximum Sum of Alternating Subsequence With Distance at Least K

Hard

You are given an integer array `nums` of length `n` and an integer `k`.

Pick a **subsequence** with indices <code>0 <= i<sub>1</sub> < i<sub>2</sub> < ... < i<sub>m</sub> < n</code> such that:

*   For every `1 <= t < m`, <code>i<sub>t+1</sub> - i<sub>t</sub> >= k</code>.
*   The selected values form a **strictly alternating** sequence. In other words, either:
    *   <code>nums[i<sub>1</sub>] < nums[i<sub>2</sub>] > nums[i<sub>3</sub>] < ...</code>, or
    *   <code>nums[i<sub>1</sub>] > nums[i<sub>2</sub>] < nums[i<sub>3</sub>] > ...</code>

A **subsequence** of length 1 is also considered **strictly** alternating. The score of a **valid** subsequence is the **sum** of its selected values.

Return an integer denoting the **maximum** possible **score** of a valid subsequence.

**Example 1:**

**Input:** nums = [5,4,2], k = 2

**Output:** 7

**Explanation:**

An optimal choice is indices `[0, 2]`, which gives values `[5, 2]`.

*   The distance condition holds because `2 - 0 = 2 >= k`.
*   The values are strictly alternating because `5 > 2`.

The score is `5 + 2 = 7`.

**Example 2:**

**Input:** nums = [3,5,4,2,4], k = 1

**Output:** 14

**Explanation:**

An optimal choice is indices `[0, 1, 3, 4]`, which gives values `[3, 5, 2, 4]`.

*   The distance condition holds because each pair of consecutive chosen indices differs by at least `k = 1`.
*   The values are strictly alternating since `3 < 5 > 2 < 4`.

The score is `3 + 5 + 2 + 4 = 14`.

**Example 3:**

**Input:** nums = [5], k = 1

**Output:** 5

**Explanation:**

The only valid subsequence is `[5]`. A subsequence with 1 element is always strictly alternating, so the score is 5.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= k <= n`

## Solution

```java
public class Solution {
    public long maxAlternatingSum(int[] nums, int k) {
        int n = nums.length;
        int maxVal = 0;
        // Find the maximum value in nums to define the size of our data structures
        for (int x : nums) {
            if (x > maxVal) {
                maxVal = x;
            }
        }
        // Variable created as requested to store an input midway in the function
        // peak[i] stores the max alternating sum ending at index i where nums[i] is a peak
        long[] peak = new long[n];
        // valley[i] stores the max alternating sum ending at index i where nums[i] is a valley
        long[] valley = new long[n];
        // Fenwick Trees for Range Maximum Queries
        // prefixBitTree will store valley[j] values to find max valley[j] where nums[j] < nums[i]
        long[] prefixBitTree = new long[maxVal + 1];
        // suffixBitTree will store peak[j] values to find max peak[j] where nums[j] > nums[i]
        // We use the transformation maxVal - nums[j] + 1 to treat suffix max as prefix max
        long[] suffixBitTree = new long[maxVal + 1];
        long maxScore = 0;
        for (int i = 0; i < n; i++) {
            // Distance condition: consecutive indices must differ by at least k
            if (i >= k) {
                // Activate the valid index (i - k) by updating both Fenwick trees
                int valIdxPrefix = nums[i - k];
                long valPrefix = valley[i - k];
                for (int idx = valIdxPrefix; idx <= maxVal; idx += idx & -idx) {
                    if (valPrefix > prefixBitTree[idx]) {
                        prefixBitTree[idx] = valPrefix;
                    }
                }
                int valIdxSuffix = maxVal - nums[i - k] + 1;
                long valSuffix = peak[i - k];
                for (int idx = valIdxSuffix; idx <= maxVal; idx += idx & -idx) {
                    if (valSuffix > suffixBitTree[idx]) {
                        suffixBitTree[idx] = valSuffix;
                    }
                }
            }
            // Find max alternating sum if nums[i] is the current peak (needs previous valley <
            // nums[i])
            long maxPrevValley = 0;
            for (int q = nums[i] - 1; q > 0; q -= q & -q) {
                if (prefixBitTree[q] > maxPrevValley) {
                    maxPrevValley = prefixBitTree[q];
                }
            }
            // A length-1 subsequence is also strictly alternating
            peak[i] = nums[i] + maxPrevValley;
            // Find max alternating sum if nums[i] is the current valley (needs previous peak >
            // nums[i])
            long maxPrevPeak = 0;
            for (int q = maxVal - nums[i]; q > 0; q -= q & -q) {
                if (suffixBitTree[q] > maxPrevPeak) {
                    maxPrevPeak = suffixBitTree[q];
                }
            }
            // A length-1 subsequence is also strictly alternating
            valley[i] = nums[i] + maxPrevPeak;
            // Update global maximum score
            if (peak[i] > maxScore) {
                maxScore = peak[i];
            }
            if (valley[i] > maxScore) {
                maxScore = valley[i];
            }
        }
        return maxScore;
    }
}
```