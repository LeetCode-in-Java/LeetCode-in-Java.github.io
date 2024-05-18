[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3131\. Find the Integer Added to Array I

Easy

You are given two arrays of equal length, `nums1` and `nums2`.

Each element in `nums1` has been increased (or decreased in the case of negative) by an integer, represented by the variable `x`.

As a result, `nums1` becomes **equal** to `nums2`. Two arrays are considered **equal** when they contain the same integers with the same frequencies.

Return the integer `x`.

**Example 1:**

**Input:** nums1 = [2,6,4], nums2 = [9,7,5]

**Output:** 3

**Explanation:**

The integer added to each element of `nums1` is 3.

**Example 2:**

**Input:** nums1 = [10], nums2 = [5]

**Output:** \-5

**Explanation:**

The integer added to each element of `nums1` is -5.

**Example 3:**

**Input:** nums1 = [1,1,1,1], nums2 = [1,1,1,1]

**Output:** 0

**Explanation:**

The integer added to each element of `nums1` is 0.

**Constraints:**

*   `1 <= nums1.length == nums2.length <= 100`
*   `0 <= nums1[i], nums2[i] <= 1000`
*   The test cases are generated in a way that there is an integer `x` such that `nums1` can become equal to `nums2` by adding `x` to each element of `nums1`.

## Solution

```java
public class Solution {
    public int addedInteger(int[] nums1, int[] nums2) {
        int n1 = nums1.length;
        int s1 = 0;
        int s2 = 0;
        for (int i : nums1) {
            s1 += i;
        }
        for (int i : nums2) {
            s2 += i;
        }
        return (s2 - s1) / n1;
    }
}
```