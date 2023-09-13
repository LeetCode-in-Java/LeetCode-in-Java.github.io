[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4\. Median of Two Sorted Arrays

Hard

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example 1:**

**Input:** nums1 = [1,3], nums2 = [2]

**Output:** 2.00000

**Explanation:** merged array = [1,2,3] and median is 2. 

**Example 2:**

**Input:** nums1 = [1,2], nums2 = [3,4]

**Output:** 2.50000

**Explanation:** merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5. 

**Example 3:**

**Input:** nums1 = [0,0], nums2 = [0,0]

**Output:** 0.00000 

**Example 4:**

**Input:** nums1 = [], nums2 = [1]

**Output:** 1.00000 

**Example 5:**

**Input:** nums1 = [2], nums2 = []

**Output:** 2.00000 

**Constraints:**

*   `nums1.length == m`
*   `nums2.length == n`
*   `0 <= m <= 1000`
*   `0 <= n <= 1000`
*   `1 <= m + n <= 2000`
*   <code>-10<sup>6</sup> <= nums1[i], nums2[i] <= 10<sup>6</sup></code>

## Solution

```java
@SuppressWarnings("java:S2234")
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums2.length < nums1.length) {
            return findMedianSortedArrays(nums2, nums1);
        }
        int cut1;
        int cut2;
        int n1 = nums1.length;
        int n2 = nums2.length;
        int low = 0;
        int high = n1;
        while (low <= high) {
            cut1 = (low + high) / 2;
            cut2 = ((n1 + n2 + 1) / 2) - cut1;
            int l1 = cut1 == 0 ? Integer.MIN_VALUE : nums1[cut1 - 1];
            int l2 = cut2 == 0 ? Integer.MIN_VALUE : nums2[cut2 - 1];
            int r1 = cut1 == n1 ? Integer.MAX_VALUE : nums1[cut1];
            int r2 = cut2 == n2 ? Integer.MAX_VALUE : nums2[cut2];
            if (l1 <= r2 && l2 <= r1) {
                if ((n1 + n2) % 2 == 0) {
                    return (Math.max(l1, l2) + Math.min(r1, r2)) / 2.0;
                }
                return Math.max(l1, l2);
            } else if (l1 > r2) {
                high = cut1 - 1;
            } else {
                low = cut1 + 1;
            }
        }
        return 0.0f;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(log(min(N, M))), where "N" and "M" represent the lengths of the input arrays `nums1` and `nums2`, respectively. Here's the breakdown:

1. The program uses a binary search algorithm to find the median of the two sorted arrays.
2. The binary search operates on the smaller of the two input arrays. This choice ensures that the search is performed on the shorter array, which reduces the number of iterations.
3. In each iteration of the binary search, the program performs constant-time operations. These operations include arithmetic calculations, comparisons, and array access.

Since the binary search reduces the search space by half in each iteration, the time complexity of this algorithm is O(log(min(N, M))), where "N" and "M" are the lengths of the input arrays `nums1` and `nums2`, respectively.

**Space Complexity (Big O Space):**

The space complexity of this program is O(1), which means it uses a constant amount of extra space that does not depend on the input size. The program uses a few integer variables (`cut1`, `cut2`, `n1`, `n2`, `low`, `high`, `l1`, `l2`, `r1`, and `r2`) to keep track of indices and values during the binary search. These variables take up a constant amount of space, regardless of the input size.

Therefore, the space complexity is O(1).
