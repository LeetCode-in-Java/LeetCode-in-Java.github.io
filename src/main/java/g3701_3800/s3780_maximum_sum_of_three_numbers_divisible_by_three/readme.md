[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3780\. Maximum Sum of Three Numbers Divisible by Three

Medium

You are given an integer array `nums`.

Your task is to choose **exactly three** integers from `nums` such that their sum is divisible by three.

Return the **maximum** possible sum of such a triplet. If no such triplet exists, return 0.

**Example 1:**

**Input:** nums = [4,2,3,1]

**Output:** 9

**Explanation:**

The valid triplets whose sum is divisible by 3 are:

*   `(4, 2, 3)` with a sum of `4 + 2 + 3 = 9`.
*   `(2, 3, 1)` with a sum of `2 + 3 + 1 = 6`.

Thus, the answer is 9.

**Example 2:**

**Input:** nums = [2,1,5]

**Output:** 0

**Explanation:**

No triplet forms a sum divisible by 3, so the answer is 0.

**Constraints:**

*   <code>3 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int maximumSum(int[] nums) {
        int[][] group = new int[3][3];
        for (int num : nums) {
            int m = num % 3;
            int max1 = group[m][0];
            int max2 = group[m][1];
            int max3 = group[m][2];
            if (num >= max1) {
                max3 = max2;
                max2 = max1;
                max1 = num;
            } else if (num >= max2) {
                max3 = max2;
                max2 = num;
            } else if (num > max3) {
                max3 = num;
            }
            group[m][0] = max1;
            group[m][1] = max2;
            group[m][2] = max3;
        }
        int res = 0;
        for (int[] g : group) {
            int sum = 0;
            for (int i = 0; i < 3; i += 1) {
                if (g[i] == 0) {
                    sum = 0;
                    break;
                }
                sum += g[i];
            }
            res = Math.max(res, sum);
        }
        int max = 0;
        for (int[] g : group) {
            if (g[0] == 0) {
                max = 0;
                break;
            }
            max += g[0];
        }
        res = Math.max(res, max);
        return res;
    }
}
```