[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4044\. Count Good Cyclic Rotations

Medium

You are given an integer array `nums` of even length `n`.

A **cyclic rotation** of `nums` is obtained by choosing a prefix of `nums` whose length is between 0 and `n - 1` (inclusive), and moving it to the end of the array while preserving the order of all elements.

A cyclic rotation is **good** if the sum of its first `n / 2` elements is **strictly greater** than the sum of its last `n / 2` elements.

Return the number of cyclic rotations of `nums` that are good.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6]

**Output:** 3

**Explanation:**

The cyclic rotations of `nums` are:

| Cyclic rotation | Sum of first `n / 2` elements | Sum of last `n / 2` elements |
| --- | --- | --- |
| `[1, 2, 3, 4, 5, 6]` | `1 + 2 + 3 = 6` | `4 + 5 + 6 = 15` |
| `[2, 3, 4, 5, 6, 1]` | `2 + 3 + 4 = 9` | `5 + 6 + 1 = 12` |
| `[3, 4, 5, 6, 1, 2]` | `3 + 4 + 5 = 12` | `6 + 1 + 2 = 9` |
| `[4, 5, 6, 1, 2, 3]` | `4 + 5 + 6 = 15` | `1 + 2 + 3 = 6` |
| `[5, 6, 1, 2, 3, 4]` | `5 + 6 + 1 = 12` | `2 + 3 + 4 = 9` |
| `[6, 1, 2, 3, 4, 5]` | `6 + 1 + 2 = 9` | `3 + 4 + 5 = 12` |

The first half has a greater sum than the second half for 3 rotations. Thus, the answer is 3.

**Example 2:**

**Input:** nums = [1,2,1,2]

**Output:** 0

**Explanation:**

The cyclic rotations of `nums` are:

| Cyclic rotation | Sum of first `n / 2` elements | Sum of last `n / 2` elements |
| --- | --- | --- |
| `[1, 2, 1, 2]` | `1 + 2 = 3` | `1 + 2 = 3` |
| `[2, 1, 2, 1]` | `2 + 1 = 3` | `2 + 1 = 3` |
| `[1, 2, 1, 2]` | `1 + 2 = 3` | `1 + 2 = 3` |
| `[2, 1, 2, 1]` | `2 + 1 = 3` | `2 + 1 = 3` |

No cyclic rotation is good because the two sums are equal for every rotation. Thus, the answer is 0.

**Constraints:**

*   <code>2 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `n` is even.

## Solution

```java
public class Solution {
    public int countGoodRotations(int[] nums) {
        long leftsum = 0;
        long rightsum = 0;
        int n = nums.length;
        int count = 0;
        for (int i = 0; i < n / 2; i++) {
            leftsum += nums[i];
            rightsum += nums[n - 1 - i];
        }
        int left = 0;
        int right = n / 2;
        while (right < n) {
            leftsum = leftsum - nums[left] + nums[right];
            rightsum = rightsum - nums[right] + nums[left];
            left++;
            right++;
            if (leftsum != rightsum) {
                count++;
            }
        }
        return count;
    }
}
```