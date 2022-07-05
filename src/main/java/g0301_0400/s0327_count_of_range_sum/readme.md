[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 327\. Count of Range Sum

Hard

Given an integer array `nums` and two integers `lower` and `upper`, return _the number of range sums that lie in_ `[lower, upper]` _inclusive_.

Range sum `S(i, j)` is defined as the sum of the elements in `nums` between indices `i` and `j` inclusive, where `i <= j`.

**Example 1:**

**Input:** nums = [-2,5,-1], lower = -2, upper = 2

**Output:** 3

**Explanation:** The three ranges are: [0,0], [2,2], and [0,2] and their respective sums are: -2, -1, 2. 

**Example 2:**

**Input:** nums = [0], lower = 0, upper = 0

**Output:** 1 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>
*   <code>-10<sup>5</sup> <= lower <= upper <= 10<sup>5</sup></code>
*   The answer is **guaranteed** to fit in a **32-bit** integer.

## Solution

```java
public class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        int n = nums.length;
        long[] sums = new long[n + 1];
        for (int i = 0; i < n; i++) {
            sums[i + 1] = sums[i] + nums[i];
        }
        return countWhileMergeSort(sums, 0, n + 1, lower, upper);
    }

    private int countWhileMergeSort(long[] sums, int start, int end, int lower, int upper) {
        if (end - start <= 1) {
            return 0;
        }
        int mid = (start + end) / 2;
        int count =
                countWhileMergeSort(sums, start, mid, lower, upper)
                        + countWhileMergeSort(sums, mid, end, lower, upper);
        int j = mid;
        int k = mid;
        int t = mid;
        long[] cache = new long[end - start];
        int r = 0;
        for (int i = start; i < mid; i++) {
            while (k < end && sums[k] - sums[i] < lower) {
                k++;
            }
            while (j < end && sums[j] - sums[i] <= upper) {
                j++;
            }
            while (t < end && sums[t] < sums[i]) {
                cache[r++] = sums[t++];
            }
            cache[r] = sums[i];
            count += j - k;
            r++;
        }
        System.arraycopy(cache, 0, sums, start, t - start);
        return count;
    }
}
```