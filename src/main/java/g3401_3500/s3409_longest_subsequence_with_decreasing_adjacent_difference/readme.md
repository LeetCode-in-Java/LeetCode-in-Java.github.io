[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3409\. Longest Subsequence With Decreasing Adjacent Difference

Medium

You are given an array of integers `nums`.

Your task is to find the length of the **longest subsequence** `seq` of `nums`, such that the **absolute differences** between _consecutive_ elements form a **non-increasing sequence** of integers. In other words, for a subsequence <code>seq<sub>0</sub></code>, <code>seq<sub>1</sub></code>, <code>seq<sub>2</sub></code>, ..., <code>seq<sub>m</sub></code> of `nums`, <code>|seq<sub>1</sub> - seq<sub>0</sub>| >= |seq<sub>2</sub> - seq<sub>1</sub>| >= ... >= |seq<sub>m</sub> - seq<sub>m - 1</sub>|</code>.

Return the length of such a subsequence.

A **subsequence** is an **non-empty** array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [16,6,3]

**Output:** 3

**Explanation:**

The longest subsequence is `[16, 6, 3]` with the absolute adjacent differences `[10, 3]`.

**Example 2:**

**Input:** nums = [6,5,3,4,2,1]

**Output:** 4

**Explanation:**

The longest subsequence is `[6, 4, 2, 1]` with the absolute adjacent differences `[2, 2, 1]`.

**Example 3:**

**Input:** nums = [10,20,10,19,10,20]

**Output:** 5

**Explanation:**

The longest subsequence is `[10, 20, 10, 19, 10]` with the absolute adjacent differences `[10, 10, 9, 9]`.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>4</sup></code>
*   `1 <= nums[i] <= 300`

## Solution

```java
public class Solution {
    public int longestSubsequence(int[] nums) {
        int max = 0;
        for (int n : nums) {
            max = Math.max(n, max);
        }
        max += 1;
        int[][] dp = new int[max][max];
        for (int i : nums) {
            int v = 1;
            for (int diff = max - 1; diff >= 0; diff--) {
                if (i + diff < max) {
                    v = Math.max(v, dp[i + diff][diff] + 1);
                }
                if (i - diff >= 0) {
                    v = Math.max(v, dp[i - diff][diff] + 1);
                }
                dp[i][diff] = v;
            }
        }
        int res = 0;
        for (int[] i : dp) {
            for (int j : i) {
                res = Math.max(res, j);
            }
        }
        return res;
    }
}
```