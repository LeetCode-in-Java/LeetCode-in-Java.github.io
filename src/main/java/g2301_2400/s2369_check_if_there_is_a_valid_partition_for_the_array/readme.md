[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2369\. Check if There is a Valid Partition For The Array

Medium

You are given a **0-indexed** integer array `nums`. You have to partition the array into one or more **contiguous** subarrays.

We call a partition of the array **valid** if each of the obtained subarrays satisfies **one** of the following conditions:

1.  The subarray consists of **exactly** `2` equal elements. For example, the subarray `[2,2]` is good.
2.  The subarray consists of **exactly** `3` equal elements. For example, the subarray `[4,4,4]` is good.
3.  The subarray consists of **exactly** `3` consecutive increasing elements, that is, the difference between adjacent elements is `1`. For example, the subarray `[3,4,5]` is good, but the subarray `[1,3,5]` is not.

Return `true` _if the array has **at least** one valid partition_. Otherwise, return `false`.

**Example 1:**

**Input:** nums = [4,4,4,5,6]

**Output:** true

**Explanation:** The array can be partitioned into the subarrays [4,4] and [4,5,6]. This partition is valid, so we return true.

**Example 2:**

**Input:** nums = [1,1,1,2]

**Output:** false

**Explanation:** There is no valid partition for this array.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public boolean validPartition(int[] nums) {
        boolean[] canPartition = new boolean[nums.length + 1];
        canPartition[0] = true;
        int diff = nums[1] - nums[0];
        boolean equal = diff == 0;
        boolean incOne = diff == 1;
        canPartition[2] = equal;
        for (int i = 3; i < canPartition.length; i++) {
            diff = nums[i - 1] - nums[i - 2];
            if (diff == 0) {
                canPartition[i] = canPartition[i - 2] || (equal && canPartition[i - 3]);
                equal = true;
                incOne = false;
            } else if (diff == 1) {
                canPartition[i] = incOne && canPartition[i - 3];
                equal = false;
                incOne = true;
            }
        }
        return canPartition[nums.length];
    }
}
```