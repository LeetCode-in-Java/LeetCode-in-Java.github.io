[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3410\. Maximize Subarray Sum After Removing All Occurrences of One Element

Hard

You are given an integer array `nums`.

You can do the following operation on the array **at most** once:

*   Choose **any** integer `x` such that `nums` remains **non-empty** on removing all occurrences of `x`.
*   Remove **all** occurrences of `x` from the array.

Return the **maximum** subarray sum across **all** possible resulting arrays.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [-3,2,-2,-1,3,-2,3]

**Output:** 7

**Explanation:**

We can have the following arrays after at most one operation:

*   The original array is <code>nums = [-3, 2, -2, -1, <ins>**3, -2, 3**</ins>]</code>. The maximum subarray sum is `3 + (-2) + 3 = 4`.
*   Deleting all occurences of `x = -3` results in <code>nums = [2, -2, -1, **<ins>3, -2, 3</ins>**]</code>. The maximum subarray sum is `3 + (-2) + 3 = 4`.
*   Deleting all occurences of `x = -2` results in <code>nums = [-3, **<ins>2, -1, 3, 3</ins>**]</code>. The maximum subarray sum is `2 + (-1) + 3 + 3 = 7`.
*   Deleting all occurences of `x = -1` results in <code>nums = [-3, 2, -2, **<ins>3, -2, 3</ins>**]</code>. The maximum subarray sum is `3 + (-2) + 3 = 4`.
*   Deleting all occurences of `x = 3` results in <code>nums = [-3, <ins>**2**</ins>, -2, -1, -2]</code>. The maximum subarray sum is 2.

The output is `max(4, 4, 7, 4, 2) = 7`.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** 10

**Explanation:**

It is optimal to not perform any operations.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>6</sup> <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public long maxSubarraySum(int[] nums) {
        Map<Long, Long> prefixMap = new HashMap<>();
        long result = nums[0];
        long prefixSum = 0;
        long minPrefix = 0;
        prefixMap.put(0L, 0L);
        for (int num : nums) {
            prefixSum += num;
            result = Math.max(result, prefixSum - minPrefix);
            if (num < 0) {
                if (prefixMap.containsKey((long) num)) {
                    prefixMap.put(
                            (long) num,
                            Math.min(prefixMap.get((long) num), prefixMap.get(0L)) + num);
                } else {
                    prefixMap.put((long) num, prefixMap.get(0L) + num);
                }
                minPrefix = Math.min(minPrefix, prefixMap.get((long) num));
            }
            prefixMap.put(0L, Math.min(prefixMap.get(0L), prefixSum));
            minPrefix = Math.min(minPrefix, prefixMap.get(0L));
        }
        return result;
    }
}
```