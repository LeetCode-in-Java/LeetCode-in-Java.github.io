[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1493\. Longest Subarray of 1's After Deleting One Element

Medium

Given a binary array `nums`, you should delete one element from it.

Return _the size of the longest non-empty subarray containing only_ `1`_'s in the resulting array_. Return `0` if there is no such subarray.

**Example 1:**

**Input:** nums = [1,1,0,1]

**Output:** 3

**Explanation:** After deleting the number in position 2, [1,1,1] contains 3 numbers with value of 1's.

**Example 2:**

**Input:** nums = [0,1,1,1,0,1,1,0,1]

**Output:** 5

**Explanation:** After deleting the number in position 4, [0,1,1,1,1,1,0,1] longest subarray with value of 1's is [1,1,1,1,1].

**Example 3:**

**Input:** nums = [1,1,1]

**Output:** 2

**Explanation:** You must delete one element.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either `0` or `1`.

## Solution

```java
public class Solution {
    public int longestSubarray(int[] nums) {
        int s = 0;
        int e = 0;
        int max = Integer.MIN_VALUE;
        boolean extraZero = false;
        boolean allOne = true;
        while (e < nums.length) {
            if (nums[e] == 1) {
                e++;
            } else if (!extraZero) {
                allOne = false;
                extraZero = true;
                e++;
            } else {
                allOne = false;
                max = Math.max(max, e - s - 1);
                while (nums[s] != 0) {
                    s++;
                }
                s++;
                extraZero = false;
            }
        }
        if (nums[e - 1] == 1) {
            max = Math.max(max, e - s - 1);
        }
        if (allOne) {
            return nums.length - 1;
        }
        return max == Integer.MIN_VALUE ? 0 : max;
    }
}
```