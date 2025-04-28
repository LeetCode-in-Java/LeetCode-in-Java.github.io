[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3533\. Concatenated Divisibility

Hard

You are given an array of positive integers `nums` and a positive integer `k`.

A permutation of `nums` is said to form a **divisible concatenation** if, when you _concatenate_ _the decimal representations_ of the numbers in the order specified by the permutation, the resulting number is **divisible by** `k`.

Return the **lexicographically smallest** permutation (when considered as a list of integers) that forms a **divisible concatenation**. If no such permutation exists, return an empty list.

**Example 1:**

**Input:** nums = [3,12,45], k = 5

**Output:** [3,12,45]

**Explanation:**

| Permutation | Concatenated Value | Divisible by 5 |
|-------------|--------------------|----------------|
| [3, 12, 45] | 31245              | Yes            |
| [3, 45, 12] | 34512              | No             |
| [12, 3, 45] | 12345              | Yes            |
| [12, 45, 3] | 12453              | No             |
| [45, 3, 12] | 45312              | No             |
| [45, 12, 3] | 45123              | No             |

The lexicographically smallest permutation that forms a divisible concatenation is `[3,12,45]`.

**Example 2:**

**Input:** nums = [10,5], k = 10

**Output:** [5,10]

**Explanation:**

| Permutation | Concatenated Value | Divisible by 10 |
|-------------|--------------------|-----------------|
| [5, 10]     | 510                | Yes             |
| [10, 5]     | 105                | No              |

The lexicographically smallest permutation that forms a divisible concatenation is `[5,10]`.

**Example 3:**

**Input:** nums = [1,2,3], k = 5

**Output:** []

**Explanation:**

Since no permutation of `nums` forms a valid divisible concatenation, return an empty list.

**Constraints:**

*   `1 <= nums.length <= 13`
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= k <= 100`

## Solution

```java
import java.util.Arrays;

@SuppressWarnings("java:S107")
public class Solution {
    public int[] concatenatedDivisibility(int[] nums, int k) {
        Arrays.sort(nums);
        int digits = 0;
        int n = nums.length;
        int[] digCnt = new int[n];
        for (int i = 0; i < n; i++) {
            int num = nums[i];
            digits++;
            digCnt[i]++;
            while (num >= 10) {
                digits++;
                digCnt[i]++;
                num /= 10;
            }
        }
        int[] pow10 = new int[digits + 1];
        pow10[0] = 1;
        for (int i = 1; i <= digits; i++) {
            pow10[i] = (pow10[i - 1] * 10) % k;
        }
        int[] res = new int[n];
        return dfs(0, 0, k, digCnt, nums, pow10, new boolean[1 << n][k], 0, res, n)
                ? res
                : new int[0];
    }

    private boolean dfs(
            int mask,
            int residue,
            int k,
            int[] digCnt,
            int[] nums,
            int[] pow10,
            boolean[][] visited,
            int ansIdx,
            int[] ans,
            int n) {
        if (ansIdx == n) {
            return residue == 0;
        }
        if (visited[mask][residue]) {
            return false;
        }
        for (int i = 0, bit = 1; i < n; i++, bit <<= 1) {
            if ((mask & bit) == bit) {
                continue;
            }
            int newResidue = (residue * pow10[digCnt[i]] + nums[i]) % k;
            ans[ansIdx] = nums[i];
            if (dfs(mask | bit, newResidue, k, digCnt, nums, pow10, visited, ansIdx + 1, ans, n)) {
                return true;
            }
        }
        visited[mask][residue] = true;
        return false;
    }
}
```