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
        long s1 = 0;
        long s2 = 0;
        long l = 0;
        long r = 0;
        for (int i : nums1) {
            s1 += i;
            if (i == 0) {
                l++;
            }
        }
        for (int i : nums2) {
            s2 += i;
            if (i == 0) {
                r++;
            }
        }
        if (s1 == s2 && l == 0 && r == 0) {
            return s1;
        }
        long x = Math.abs(s1 - s2);
        if (s1 > s2) {
            if (r == 0 || (l == 0 && r > x)) {
                return -1;
            }
            if (l == 0) {
                return s1;
            }
            return s1 + Math.max(l, r - x);
        } else {
            if (l == 0 || (r == 0 && l > x)) {
                return -1;
            }
            if (s1 == s2) {
                return s1 + Math.max(l, r);
            }
            if (r == 0) {
                return s2;
            }
            return s2 + Math.max(r, l - x);
        }
    }
}
```