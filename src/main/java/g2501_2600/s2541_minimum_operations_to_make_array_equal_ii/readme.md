[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2541\. Minimum Operations to Make Array Equal II

Medium

You are given two integer arrays `nums1` and `nums2` of equal length `n` and an integer `k`. You can perform the following operation on `nums1`:

*   Choose two indexes `i` and `j` and increment `nums1[i]` by `k` and decrement `nums1[j]` by `k`. In other words, `nums1[i] = nums1[i] + k` and `nums1[j] = nums1[j] - k`.

`nums1` is said to be **equal** to `nums2` if for all indices `i` such that `0 <= i < n`, `nums1[i] == nums2[i]`.

Return _the **minimum** number of operations required to make_ `nums1` _equal to_ `nums2`. If it is impossible to make them equal, return `-1`.

**Example 1:**

**Input:** nums1 = [4,3,1,4], nums2 = [1,3,7,1], k = 3

**Output:** 2

**Explanation:** In 2 operations, we can transform nums1 to nums2. 

1<sup>st</sup> operation: i = 2, j = 0. After applying the operation, nums1 = [1,3,4,4]. 

2<sup>nd</sup> operation: i = 2, j = 3. After applying the operation, nums1 = [1,3,7,1]. One can prove that it is impossible to make arrays equal in fewer operations.

**Example 2:**

**Input:** nums1 = [3,8,5,2], nums2 = [2,4,1,6], k = 1

**Output:** -1

**Explanation:** It can be proved that it is impossible to make the two arrays equal.

**Constraints:**

*   `n == nums1.length == nums2.length`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>0 <= nums1[i], nums2[j] <= 10<sup>9</sup></code>
*   <code>0 <= k <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public long minOperations(int[] nums1, int[] nums2, int k) {
        int n = nums1.length;
        long pcnt = 0;
        long ncnt = 0;
        if (k == 0) {
            if (Arrays.equals(nums1, nums2)) {
                return 0;
            } else {
                return -1;
            }
        }
        for (int i = 0; i < n; i++) {
            int tp = nums1[i] - nums2[i];
            if (tp % k != 0) {
                return -1;
            }
            if (tp > 0) {
                pcnt += tp;
            } else if (tp < 0) {
                ncnt += tp;
            }
        }
        if (pcnt + ncnt != 0) {
            return -1;
        }
        return pcnt / k;
    }
}
```