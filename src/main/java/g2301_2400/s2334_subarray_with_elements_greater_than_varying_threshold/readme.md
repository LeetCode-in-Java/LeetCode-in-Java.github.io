[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2334\. Subarray With Elements Greater Than Varying Threshold

Hard

You are given an integer array `nums` and an integer `threshold`.

Find any subarray of `nums` of length `k` such that **every** element in the subarray is **greater** than `threshold / k`.

Return _the **size** of **any** such subarray_. If there is no such subarray, return `-1`.

A **subarray** is a contiguous non-empty sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,3,4,3,1], threshold = 6

**Output:** 3

**Explanation:** The subarray [3,4,3] has a size of 3, and every element is greater than 6 / 3 = 2.

Note that this is the only valid subarray. 

**Example 2:**

**Input:** nums = [6,5,6,5,8], threshold = 7

**Output:** 1

**Explanation:** The subarray [8] has a size of 1, and 8 > 7 / 1 = 7. So 1 is returned.

Note that the subarray [6,5] has a size of 2, and every element is greater than 7 / 2 = 3.5.

Similarly, the subarrays [6,5,6], [6,5,6,5], [6,5,6,5,8] also satisfy the given conditions.

Therefore, 2, 3, 4, or 5 may also be returned.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], threshold <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.PriorityQueue;
import java.util.TreeSet;

public class Solution {
    public int validSubarraySize(int[] nums, int threshold) {
        int n = nums.length;
        int[] min = new int[n];
        // base case
        TreeSet<Integer> dead = new TreeSet<>(Arrays.asList(n, -1));
        PriorityQueue<Integer> maxheap = new PriorityQueue<>(Comparator.comparingInt(o -> -min[o]));
        for (int i = 0; i < n; i++) {
            min[i] = threshold / nums[i] + 1;
            if (min[i] > nums.length) {
                // dead, this element should never appear in the answer
                dead.add(i);
            } else {
                maxheap.offer(i);
            }
        }
        while (!maxheap.isEmpty()) {
            int cur = maxheap.poll();
            if (dead.higher(cur) - dead.lower(cur) - 1 < min[cur]) {
                // widest open range < minimum required length, this index is also bad.
                dead.add(cur);
            } else {
                // otherwise we've found it!
                return min[cur];
            }
        }
        return -1;
    }
}
```