[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2918\. Minimum Equal Sum of Two Arrays After Replacing Zeros

Medium

You are given two arrays `nums1` and `nums2` consisting of positive integers.

You have to replace **all** the `0`'s in both arrays with **strictly** positive integers such that the sum of elements of both arrays becomes **equal**.

Return _the **minimum** equal sum you can obtain, or_ `-1` _if it is impossible_.

**Example 1:**

**Input:** nums1 = [3,2,0,1,0], nums2 = [6,5,0]

**Output:** 12

**Explanation:** We can replace 0's in the following way: 
- Replace the two 0's in nums1 with the values 2 and 4. The resulting array is nums1 = [3,2,2,1,4]. 
- Replace the 0 in nums2 with the value 1. The resulting array is nums2 = [6,5,1]. Both arrays have an equal sum of 12. It can be shown that it is the minimum sum we can obtain.

**Example 2:**

**Input:** nums1 = [2,0,2,0], nums2 = [1,4]

**Output:** -1

**Explanation:** It is impossible to make the sum of both arrays equal.

**Constraints:**

*   <code>1 <= nums1.length, nums2.length <= 10<sup>5</sup></code>
*   <code>0 <= nums1[i], nums2[i] <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public long minSum(int[] nums1, int[] nums2) {
        long zero1Count = 0;
        long sum1 = 0;
        long zero2Count = 0;
        long sum2 = 0;
        for (int num : nums1) {
            zero1Count += num == 0 ? 1 : 0;
            sum1 += num;
        }
        for (int num : nums2) {
            zero2Count += num == 0 ? 1 : 0;
            sum2 += num;
        }
        if (zero1Count == 0 && zero2Count == 0) {
            return sum1 == sum2 ? sum1 : -1;
        }
        if (zero1Count == 0) {
            return (sum1 - sum2 >= zero2Count) ? sum1 : -1;
        }
        if (zero2Count == 0) {
            return (sum2 - sum1 >= zero1Count) ? sum2 : -1;
        }
        long ans = Long.MAX_VALUE;
        long p1 = zero1Count;
        long p2 = zero1Count - (sum2 - sum1);
        if (p2 >= zero2Count) {
            ans = Math.min(ans, sum1 + p1);
        }
        p1 = (sum2 - sum1) + zero2Count;
        if (p1 >= zero1Count) {
            ans = Math.min(ans, sum1 + p1);
        }
        return ans != Long.MAX_VALUE ? ans : -1;
    }
}
```