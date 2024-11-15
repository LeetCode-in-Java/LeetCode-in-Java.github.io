[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3347\. Maximum Frequency of an Element After Performing Operations II

Hard

You are given an integer array `nums` and two integers `k` and `numOperations`.

You must perform an **operation** `numOperations` times on `nums`, where in each operation you:

*   Select an index `i` that was **not** selected in any previous operations.
*   Add an integer in the range `[-k, k]` to `nums[i]`.

Return the **maximum** possible frequency of any element in `nums` after performing the **operations**.

**Example 1:**

**Input:** nums = [1,4,5], k = 1, numOperations = 2

**Output:** 2

**Explanation:**

We can achieve a maximum frequency of two by:

*   Adding 0 to `nums[1]`, after which `nums` becomes `[1, 4, 5]`.
*   Adding -1 to `nums[2]`, after which `nums` becomes `[1, 4, 4]`.

**Example 2:**

**Input:** nums = [5,11,20,20], k = 5, numOperations = 1

**Output:** 2

**Explanation:**

We can achieve a maximum frequency of two by:

*   Adding 0 to `nums[1]`.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>0 <= k <= 10<sup>9</sup></code>
*   `0 <= numOperations <= nums.length`

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxFrequency(int[] nums, int k, int numOperations) {
        Arrays.sort(nums);
        int n = nums.length;
        int l = 0;
        int r = 0;
        int i = 0;
        int j = 0;
        int res = 0;
        while (i < n) {
            while (j < n && nums[j] == nums[i]) {
                j++;
            }
            while (l < i && nums[i] - nums[l] > k) {
                l++;
            }
            while (r < n && nums[r] - nums[i] <= k) {
                r++;
            }
            res = Math.max(res, Math.min(i - l + r - j, numOperations) + j - i);
            i = j;
        }
        i = 0;
        j = 0;
        while (i < n && j < n) {
            while (j < n && j - i < numOperations && nums[j] - nums[i] <= k * 2) {
                j++;
            }
            res = Math.max(res, j - i);
            i++;
        }
        return res;
    }
}
```