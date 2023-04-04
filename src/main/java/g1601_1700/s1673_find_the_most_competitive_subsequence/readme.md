[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1673\. Find the Most Competitive Subsequence

Medium

Given an integer array `nums` and a positive integer `k`, return _the most **competitive** subsequence of_ `nums` _of size_ `k`.

An array's subsequence is a resulting sequence obtained by erasing some (possibly zero) elements from the array.

We define that a subsequence `a` is more **competitive** than a subsequence `b` (of the same length) if in the first position where `a` and `b` differ, subsequence `a` has a number **less** than the corresponding number in `b`. For example, `[1,3,4]` is more competitive than `[1,3,5]` because the first position they differ is at the final number, and `4` is less than `5`.

**Example 1:**

**Input:** nums = [3,5,2,6], k = 2

**Output:** [2,6]

**Explanation:** Among the set of every possible subsequence: {[3,5], [3,2], [3,6], [5,2], [5,6], [2,6]}, [2,6] is the most competitive.

**Example 2:**

**Input:** nums = [2,4,3,3,5,4,9,6], k = 4

**Output:** [2,3,3,4]

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= nums.length`

## Solution

```java
public class Solution {
    public int[] mostCompetitive(int[] nums, int k) {
        int[] r = new int[k];
        int n = nums.length;
        int j = 0;
        for (int i = 0; i < n; i++) {
            if (i == 0) {
                r[j] = nums[i];
                j++;
            } else {
                int l = j - 1;
                while ((l >= 0) && (nums[i] < r[l]) && ((n - i) >= (k - l))) {
                    l--;
                }
                j = l + 1;
                if (j < k) {
                    r[j] = nums[i];
                    j++;
                }
            }
        }
        return r;
    }
}
```