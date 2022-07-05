[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 152\. Maximum Product Subarray

Medium

Given an integer array `nums`, find a contiguous non-empty subarray within the array that has the largest product, and return _the product_.

It is **guaranteed** that the answer will fit in a **32-bit** integer.

A **subarray** is a contiguous subsequence of the array.

**Example 1:**

**Input:** nums = [2,3,-2,4]

**Output:** 6

**Explanation:** [2,3] has the largest product 6. 

**Example 2:**

**Input:** nums = [-2,0,-1]

**Output:** 0

**Explanation:** The result cannot be 2, because [-2,-1] is not a subarray. 

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   `-10 <= nums[i] <= 10`
*   The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

## Solution

```java
public class Solution {
    public int maxProduct(int[] arr) {
        int ans = Integer.MIN_VALUE;
        int cprod = 1;
        for (int j : arr) {
            cprod = cprod * j;
            ans = Math.max(ans, cprod);
            if (cprod == 0) {
                cprod = 1;
            }
        }
        cprod = 1;
        for (int i = arr.length - 1; i >= 0; i--) {
            cprod = cprod * arr[i];
            ans = Math.max(ans, cprod);
            if (cprod == 0) {
                cprod = 1;
            }
        }
        return ans;
    }
}
```