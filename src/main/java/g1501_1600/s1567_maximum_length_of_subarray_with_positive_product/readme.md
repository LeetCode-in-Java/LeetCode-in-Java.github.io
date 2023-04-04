[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1567\. Maximum Length of Subarray With Positive Product

Medium

Given an array of integers `nums`, find the maximum length of a subarray where the product of all its elements is positive.

A subarray of an array is a consecutive sequence of zero or more values taken out of that array.

Return _the maximum length of a subarray with positive product_.

**Example 1:**

**Input:** nums = [1,-2,-3,4]

**Output:** 4

**Explanation:** The array nums already has a positive product of 24.

**Example 2:**

**Input:** nums = [0,1,-2,-3,-4]

**Output:** 3

**Explanation:** The longest subarray with positive product is [1,-2,-3] which has a product of 6. Notice that we cannot include 0 in the subarray since that'll make the product 0 which is not positive.

**Example 3:**

**Input:** nums = [-1,-2,-3,0,1]

**Output:** 2

**Explanation:** The longest subarray with positive product is [-1,-2] or [-2,-3].

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int getMaxLen(int[] nums) {
        int posLen = 0;
        int negLen = 0;
        int res = 0;
        for (int num : nums) {
            if (num == 0) {
                posLen = 0;
                negLen = 0;
            } else if (num > 0) {
                posLen++;
                negLen = negLen == 0 ? 0 : negLen + 1;
            } else {
                int temp = posLen;
                posLen = negLen == 0 ? 0 : negLen + 1;
                negLen = temp + 1;
            }
            res = Math.max(res, posLen);
        }
        return res;
    }
}
```