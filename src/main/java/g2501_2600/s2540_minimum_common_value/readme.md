[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2540\. Minimum Common Value

Easy

Given two integer arrays `nums1` and `nums2`, sorted in non-decreasing order, return _the **minimum integer common** to both arrays_. If there is no common integer amongst `nums1` and `nums2`, return `-1`.

Note that an integer is said to be **common** to `nums1` and `nums2` if both arrays have **at least one** occurrence of that integer.

**Example 1:**

**Input:** nums1 = [1,2,3], nums2 = [2,4]

**Output:** 2

**Explanation:** The smallest element common to both arrays is 2, so we return 2.

**Example 2:**

**Input:** nums1 = [1,2,3,6], nums2 = [2,3,4,5]

**Output:** 2

**Explanation:** There are two common elements in the array 2 and 3 out of which 2 is the smallest, so 2 is returned.

**Constraints:**

*   <code>1 <= nums1.length, nums2.length <= 10<sup>5</sup></code>
*   <code>1 <= nums1[i], nums2[j] <= 10<sup>9</sup></code>
*   Both `nums1` and `nums2` are sorted in **non-decreasing** order.

## Solution

```java
public class Solution {
    public int getCommon(int[] nums1, int[] nums2) {
        int i = 0;
        int j = 0;
        if (nums1[0] > nums2[nums2.length - 1] || nums1[nums1.length - 1] < nums2[0]) {
            return -1;
        }
        while (i < nums1.length && j < nums2.length) {
            if (nums1[i] == nums2[j]) {
                return nums1[i];
            }
            if (nums1[i] > nums2[j]) {
                j++;
            } else {
                i++;
            }
        }
        return -1;
    }
}
```