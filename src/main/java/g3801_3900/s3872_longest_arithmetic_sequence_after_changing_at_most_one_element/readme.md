[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3872\. Longest Arithmetic Sequence After Changing At Most One Element

Medium

You are given an integer array `nums`.

A subarray is **arithmetic** if the difference between consecutive elements in the subarray is constant.

You can replace **at most one** element in `nums` with any **integer**. Then, you select an arithmetic subarray from `nums`.

Return an integer denoting the **maximum** length of the arithmetic subarray you can select.

**Example 1:**

**Input:** nums = [9,7,5,10,1]

**Output:** 5

**Explanation:**

*   Replace `nums[3] = 10` with 3. The array becomes `[9, 7, 5, 3, 1]`.
*   Select the subarray <code>[<ins>**9, 7, 5, 3, 1**</ins>]</code>, which is arithmetic because consecutive elements have a common difference of -2.

**Example 2:**

**Input:** nums = [1,2,6,7]

**Output:** 3

**Explanation:**

*   Replace `nums[0] = 1` with -2. The array becomes `[-2, 2, 6, 7]`.
*   Select the subarray <code>[<ins>**-2, 2, 6**</ins>, 7]</code>, which is arithmetic because consecutive elements have a common difference of 4.

**Constraints:**

*   <code>4 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int longestArithmetic(int[] nums) {
        int ans = solve(nums);
        reverse(nums);
        return Math.max(ans, solve(nums));
    }

    private static int solve(int[] nums) {
        int n = nums.length;
        int max = 2;
        int diff = nums[1] - nums[0];
        int left = 0;
        for (int right = 2; right < n; right++) {
            if (nums[right] - nums[right - 1] == diff) {
                max = Math.max(max, right - left + 1);
                continue;
            }
            int temp = right;
            int val = nums[right - 1] + diff;
            while (temp + 1 < n && nums[temp + 1] - val == diff) {
                val = nums[++temp];
            }
            max = Math.max(max, temp - left + 1);
            left = right - 1;
            diff = nums[right] - nums[right - 1];
        }
        return max;
    }

    private static void reverse(int[] nums) {
        int i = 0;
        int j = nums.length - 1;
        while (i < j) {
            int temp = nums[i];
            nums[i++] = nums[j];
            nums[j--] = temp;
        }
    }
}
```