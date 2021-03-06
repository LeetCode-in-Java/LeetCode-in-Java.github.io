[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 724\. Find Pivot Index

Easy

Given an array of integers `nums`, calculate the **pivot index** of this array.

The **pivot index** is the index where the sum of all the numbers **strictly** to the left of the index is equal to the sum of all the numbers **strictly** to the index's right.

If the index is on the left edge of the array, then the left sum is `0` because there are no elements to the left. This also applies to the right edge of the array.

Return _the **leftmost pivot index**_. If no such index exists, return -1.

**Example 1:**

**Input:** nums = [1,7,3,6,5,6]

**Output:** 3

**Explanation:** 

The pivot index is 3. 

Left sum = nums[0] + nums[1] + nums[2] = 1 + 7 + 3 = 11 

Right sum = nums[4] + nums[5] = 5 + 6 = 11

**Example 2:**

**Input:** nums = [1,2,3]

**Output:** -1

**Explanation:** 

There is no index that satisfies the conditions in the problem statement.

**Example 3:**

**Input:** nums = [2,1,-1]

**Output:** 0

**Explanation:** 

The pivot index is 0. 

Left sum = 0 (no elements to the left of index 0) 

Right sum = nums[1] + nums[2] = 1 + -1 = 0

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   `-1000 <= nums[i] <= 1000`

**Note:** This question is the same as 1991: [https://leetcode.com/problems/find-the-middle-index-in-array/](https://leetcode.com/problems/find-the-middle-index-in-array/)

## Solution

```java
public class Solution {
    public int pivotIndex(int[] nums) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        int[] sums = new int[nums.length];
        sums[0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            sums[i] = sums[i - 1] + nums[i];
        }
        for (int i = 0; i < nums.length; i++) {
            int temp = sums[nums.length - 1] - sums[i];
            if (i == 0 && 0 == temp || (i > 0 && sums[i - 1] == temp)) {
                return i;
            }
        }
        return -1;
    }
}
```