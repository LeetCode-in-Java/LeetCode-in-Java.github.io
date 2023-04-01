[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1685\. Sum of Absolute Differences in a Sorted Array

Medium

You are given an integer array `nums` sorted in **non-decreasing** order.

Build and return _an integer array_ `result` _with the same length as_ `nums` _such that_ `result[i]` _is equal to the **summation of absolute differences** between_ `nums[i]` _and all the other elements in the array._

In other words, `result[i]` is equal to `sum(|nums[i]-nums[j]|)` where `0 <= j < nums.length` and `j != i` (**0-indexed**).

**Example 1:**

**Input:** nums = [2,3,5]

**Output:** [4,3,5]

**Explanation:** Assuming the arrays are 0-indexed, then

result[0] = \|2-2\| + \|2-3\| + \|2-5\| = 0 + 1 + 3 = 4,

result[1] = \|3-2\| + \|3-3\| + \|3-5\| = 1 + 0 + 2 = 3,

result[2] = \|5-2\| + \|5-3\| + \|5-5\| = 3 + 2 + 0 = 5.

**Example 2:**

**Input:** nums = [1,4,6,8,10]

**Output:** [24,15,13,15,21]

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= nums[i + 1] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int[] getSumAbsoluteDifferences(int[] nums) {
        int len = nums.length;
        int[] preSums = new int[len];
        for (int i = 1; i < len; i++) {
            preSums[i] = preSums[i - 1] + nums[i - 1];
        }
        int[] postSums = new int[len];
        for (int i = len - 2; i >= 0; i--) {
            postSums[i] = postSums[i + 1] + nums[i + 1];
        }
        int[] result = new int[len];
        for (int i = 0; i < len; i++) {
            result[i] = nums[i] * i - preSums[i] + postSums[i] - nums[i] * (len - i - 1);
        }
        return result;
    }
}
```