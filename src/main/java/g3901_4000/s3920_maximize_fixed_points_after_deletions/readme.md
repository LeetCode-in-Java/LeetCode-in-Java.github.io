[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3920\. Maximize Fixed Points After Deletions

Hard

You are given an integer array `nums`.

A position `i` is called a **fixed point** if `nums[i] == i`.

You are allowed to delete **any** number of elements (including zero) from the array. After each deletion, the remaining elements **shift left**, and indices are reassigned starting from 0.

Return an integer denoting the **maximum** number of fixed points that can be achieved after performing any number of deletions.

**Example 1:**

**Input:** nums = [0,2,1]

**Output:** 2

**Explanation:**

*   Delete `nums[1] = 2`. The array becomes `[0, 1]`.
*   Now, `nums[0] = 0` and `nums[1] = 1`, so both indices are fixed points.
*   Thus, the answer is 2.

**Example 2:**

**Input:** nums = [3,1,2]

**Output:** 2

**Explanation:**

*   Do not delete any elements. The array remains `[3, 1, 2]`.
*   Here, `nums[1] = 1` and `nums[2] = 2`, so these indices are fixed points.
*   Thus, the answer is 2.

**Example 3:**

**Input:** nums = [1,0,1,2]

**Output:** 3

**Explanation:**

*   Delete `nums[0] = 1`. The array becomes `[0, 1, 2]`.
*   Now, `nums[0] = 0`, `nums[1] = 1`, and `nums[2] = 2`, so all indices are fixed points.
*   Thus, the answer is 3.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxFixedPoints(int[] nums) {
        int n = nums.length;
        long[] arr = new long[n];
        int index = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] <= i) {
                arr[index++] = (long) i - nums[i] << 32 | nums[i];
            }
        }
        if (index == 0) {
            return 0;
        }
        Arrays.sort(arr, 0, index);
        int max = 0;
        int[] lis = new int[index];
        lis[0] = (int) arr[0];
        for (int i = 1; i < index; i++) {
            int val = (int) arr[i];
            lis[val > lis[max] ? ++max : binarySearch(lis, val, max)] = val;
        }
        return max + 1;
    }

    private int binarySearch(int[] arr, int target, int right) {
        int left = 0;
        while (left < right) {
            int mid = left + right >>> 1;
            if (arr[mid] >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```