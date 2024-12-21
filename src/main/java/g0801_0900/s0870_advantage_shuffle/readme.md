[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 870\. Advantage Shuffle

Medium

You are given two integer arrays `nums1` and `nums2` both of the same length. The **advantage** of `nums1` with respect to `nums2` is the number of indices `i` for which `nums1[i] > nums2[i]`.

Return _any permutation of_ `nums1` _that maximizes its **advantage** with respect to_ `nums2`.

**Example 1:**

**Input:** nums1 = [2,7,11,15], nums2 = [1,10,4,11]

**Output:** [2,11,7,15]

**Example 2:**

**Input:** nums1 = [12,24,8,32], nums2 = [13,25,32,11]

**Output:** [24,32,8,12]

**Constraints:**

*   <code>1 <= nums1.length <= 10<sup>5</sup></code>
*   `nums2.length == nums1.length`
*   <code>0 <= nums1[i], nums2[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public int[] advantageCount(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        int[] result = new int[nums1.length];
        int low = 0;
        boolean[] chosen = new boolean[nums1.length];
        for (int i = 0; i < nums2.length; i++) {
            int pos = binSearch(nums1, nums2[i], low, chosen);
            if (pos != -1 && pos < nums1.length) {
                result[i] = nums1[pos];
                chosen[pos] = true;
            } else {
                result[i] = -1;
            }
        }
        List<Integer> pos = new ArrayList<>();
        int i = 0;
        for (boolean ch : chosen) {
            if (!ch) {
                pos.add(i);
            }
            i++;
        }
        int index = 0;
        for (i = 0; i < result.length; i++) {
            if (result[i] == -1) {
                result[i] = nums1[pos.get(index)];
                index++;
            }
        }
        return result;
    }

    private int binSearch(int[] nums, int target, int low, boolean[] chosen) {
        int high = nums.length - 1;
        while (high >= low) {
            int mid = high - (high - low) / 2;
            if (nums[mid] > target && (mid == 0 || nums[mid - 1] <= target)) {
                while (mid < nums.length && chosen[mid]) {
                    mid++;
                }
                return mid;
            }
            if (nums[mid] > target) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }
        return -1;
    }
}
```