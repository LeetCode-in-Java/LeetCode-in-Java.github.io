[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3942\. Minimum Operations to Sort a Permutation

Medium

You are given an integer array `nums` of length `n`, where `nums` is a permutation of the integers from 0 to `n - 1`.

You may perform **only** the following operations:

*   **Reverse** the entire array.
*   **Rotate Left by One**: Move the first element to the end of the array, and rest elements to left by one position.

Return an integer denoting the **minimum** number of operations required to sort the array in **increasing** order. If it is **not possible** to sort the array using only the given operations, return -1.

**Example 1:**

**Input:** nums = [0,2,1]

**Output:** 2

**Explanation:**

*   Rotate Left by one: `[2, 1, 0]`
*   Reverse the array: `[0, 1, 2]`

The array becomes sorted in 2 operations, which is minimal

**Example 2:**

**Input:** nums = [1,0,2]

**Output:** 2

**Explanation:**

*   Reverse the array: `[2, 0, 1]`
*   Rotate Left by one: `[0, 1, 2]`

The array becomes sorted in 2 operations, which is minimal.

**Example 3:**

**Input:** nums = [2,0,1,3]

**Output:** \-1

**Explanation:**

It is impossible to reach `[2, 0, 1, 3]`. Thus, the answer is -1.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   `0 <= nums[i] <= n - 1`
*   `nums` is a permutation of integers from 0 to `n - 1`.

## Solution

```java
public class Solution {
    public int minOperations(int[] nums) {
        int ans = -1;
        int zeroIdx = 0;
        int prev = nums[0];
        boolean forward = true;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == 0) {
                zeroIdx = i;
                // new start
            } else if (nums[i] != (prev + 1)) {
                forward = false;
                break;
            }
            prev = nums[i];
        }
        if (forward) {
            ans = Math.min(zeroIdx, nums.length - zeroIdx + 2);
        }
        zeroIdx = nums.length - 1;
        prev = nums[nums.length - 1];
        boolean backward = true;
        for (int i = nums.length - 2; i >= 0; i--) {
            if (nums[i] == 0) {
                zeroIdx = i;
                // new start
            } else if (nums[i] != (prev + 1)) {
                backward = false;
                break;
            }
            prev = nums[i];
        }
        if (backward) {
            int b = Math.min(nums.length - zeroIdx, zeroIdx + 2);
            if (ans == -1) {
                ans = b;
            } else {
                ans = Math.min(ans, b);
            }
        }
        return ans;
    }
}
```