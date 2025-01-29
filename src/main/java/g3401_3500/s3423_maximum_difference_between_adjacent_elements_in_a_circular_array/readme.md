[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3423\. Maximum Difference Between Adjacent Elements in a Circular Array

Easy

Given a **circular** array `nums`, find the **maximum** absolute difference between adjacent elements.

**Note**: In a circular array, the first and last elements are adjacent.

**Example 1:**

**Input:** nums = [1,2,4]

**Output:** 3

**Explanation:**

Because `nums` is circular, `nums[0]` and `nums[2]` are adjacent. They have the maximum absolute difference of `|4 - 1| = 3`.

**Example 2:**

**Input:** nums = [-5,-10,-5]

**Output:** 5

**Explanation:**

The adjacent elements `nums[0]` and `nums[1]` have the maximum absolute difference of `|-5 - (-10)| = 5`.

**Constraints:**

*   `2 <= nums.length <= 100`
*   `-100 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    public int maxAdjacentDistance(int[] nums) {
        int maxDiff = 0;
        for (int i = 0; i < nums.length; i++) {
            int nextIndex = (i + 1) % nums.length;
            int diff = Math.abs(nums[i] - nums[nextIndex]);
            maxDiff = Math.max(maxDiff, diff);
        }
        return maxDiff;
    }
}
```