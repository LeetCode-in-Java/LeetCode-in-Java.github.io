[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3132\. Find the Integer Added to Array II

Medium

You are given two integer arrays `nums1` and `nums2`.

From `nums1` two elements have been removed, and all other elements have been increased (or decreased in the case of negative) by an integer, represented by the variable `x`.

As a result, `nums1` becomes **equal** to `nums2`. Two arrays are considered **equal** when they contain the same integers with the same frequencies.

Return the **minimum** possible integer `x` that achieves this equivalence.

**Example 1:**

**Input:** nums1 = [4,20,16,12,8], nums2 = [14,18,10]

**Output:** \-2

**Explanation:**

After removing elements at indices `[0,4]` and adding -2, `nums1` becomes `[18,14,10]`.

**Example 2:**

**Input:** nums1 = [3,5,5,3], nums2 = [7,7]

**Output:** 2

**Explanation:**

After removing elements at indices `[0,3]` and adding 2, `nums1` becomes `[7,7]`.

**Constraints:**

*   `3 <= nums1.length <= 200`
*   `nums2.length == nums1.length - 2`
*   `0 <= nums1[i], nums2[i] <= 1000`
*   The test cases are generated in a way that there is an integer `x` such that `nums1` can become equal to `nums2` by removing two elements and adding `x` to each element of `nums1`.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minimumAddedInteger(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        if (checkOk(nums1, nums2, 2)) {
            return nums2[0] - nums1[2];
        } else if (checkOk(nums1, nums2, 1)) {
            return nums2[0] - nums1[1];
        } else {
            return nums2[0] - nums1[0];
        }
    }

    private boolean checkOk(int[] nums1, int[] nums2, int start) {
        int i = 0;
        int diff = nums2[i] - nums1[start];
        while (i < nums2.length) {
            if (start - i > 2) {
                return false;
            }
            if (nums2[i] == nums1[start] + diff) {
                i++;
            }
            start++;
        }
        return i == nums2.length;
    }
}
```