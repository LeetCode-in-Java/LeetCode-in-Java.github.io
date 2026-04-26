[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3738\. Longest Non-Decreasing Subarray After Replacing at Most One Element

Medium

You are given an integer array `nums`.

You are allowed to replace **at most** one element in the array with any other integer value of your choice.

Return the length of the **longest non-decreasing subarray** that can be obtained after performing at most one replacement.

An array is said to be **non-decreasing** if each element is greater than or equal to its previous one (if it exists).

**Example 1:**

**Input:** nums = [1,2,3,1,2]

**Output:** 4

**Explanation:**

Replacing `nums[3] = 1` with 3 gives the array [1, 2, 3, 3, 2].

The longest non-decreasing subarray is [1, 2, 3, 3], which has a length of 4.

**Example 2:**

**Input:** nums = [2,2,2,2,2]

**Output:** 5

**Explanation:**

All elements in `nums` are equal, so it is already non-decreasing and the entire `nums` forms a subarray of length 5.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int longestSubarray(int[] nums) {
        int n = nums.length;
        if (n < 3) {
            return n;
        }
        int i = 0;
        int len1 = 0;
        int len2 = 0;
        int ans = 2;
        while (i < n) {
            int l = (i == 0) ? -1000 * 1000 * 10 : nums[i - 1];
            int r = (i == n - 1) ? 1000 * 1000 * 10 : nums[i + 1];
            if (l <= nums[i]) {
                len2++;
                ans = Math.max(len2 + 1, ans);
            } else if (r >= nums[i]) {
                int j = i;
                len1 = len2;
                while (j < n - 1 && nums[j] <= nums[j + 1]) {
                    j++;
                }
                len2 = j - i + 1;
                ans = Math.max(len2 + 1, ans);
                if (l <= r || (len1 > 1 && nums[i - 2] <= nums[i])) {
                    ans = Math.max(len1 + len2, ans);
                }
                i = j;
            } else {
                len2 = 0;
            }
            i++;
        }
        ans = Math.min(ans, n);
        return ans;
    }
}
```