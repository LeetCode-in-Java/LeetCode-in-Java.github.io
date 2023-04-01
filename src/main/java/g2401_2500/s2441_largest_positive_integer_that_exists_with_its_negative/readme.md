[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2441\. Largest Positive Integer That Exists With Its Negative

Easy

Given an integer array `nums` that **does not contain** any zeros, find **the largest positive** integer `k` such that `-k` also exists in the array.

Return _the positive integer_ `k`. If there is no such integer, return `-1`.

**Example 1:**

**Input:** nums = [-1,2,-3,3]

**Output:** 3

**Explanation:** 3 is the only valid k we can find in the array.

**Example 2:**

**Input:** nums = [-1,10,6,7,-7,1]

**Output:** 7

**Explanation:** Both 1 and 7 have their corresponding negative values in the array. 7 has a larger value.

**Example 3:**

**Input:** nums = [-10,8,6,7,-2,-3]

**Output:** -1

**Explanation:** There is no a single valid k, we return -1.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `-1000 <= nums[i] <= 1000`
*   `nums[i] != 0`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int findMaxK(int[] nums) {
        int[] arr = new int[nums.length];
        int j = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] < 0) {
                arr[j++] = nums[i];
            }
        }
        Arrays.sort(arr);
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            for (int num : nums) {
                if ((arr[i] * -1) == num) {
                    return num;
                }
            }
        }
        return -1;
    }
}
```