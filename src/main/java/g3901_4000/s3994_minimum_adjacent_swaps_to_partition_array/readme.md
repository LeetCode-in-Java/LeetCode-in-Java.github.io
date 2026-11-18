[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3994\. Minimum Adjacent Swaps to Partition Array

Medium

You are given an integer array `nums` and two integers `a` and `b` such that `a < b`.

An array is called **good** if it can be split into three **contiguous** parts, in this order, such that:

*   Every element in the first part is **less than** `a`.
*   Every element in the second part is **in** the range `[a, b]` inclusive.
*   Every element in the third part is **greater than** `b`.

Any of the three parts **may be** empty.

In one **adjacent swap**, you may swap two **neighboring** elements of `nums`.

Return the **minimum number** of adjacent swaps required to make `nums` good. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

OriginalJava**Example 1:**

**Input:** nums = [1,3,2,4,5,6], a = 3, b = 4

**Output:** 1

**Explanation:**

*   Swap `nums[1]` and `nums[2]`. The array becomes `[1, 2, 3, 4, 5, 6]`.
*   This array is good because it can be split into `[1, 2]`, `[3, 4]`, and `[5, 6]`.

**Example 2:**

**Input:** nums = [9,7,5,3], a = 4, b = 8

**Output:** 5

**Explanation:**

One sequence of optimal swaps is as follows:

*   Swap `nums[2]` and `nums[3]`. The array becomes `[9, 7, 3, 5]`.
*   Swap `nums[1]` and `nums[2]`. The array becomes `[9, 3, 7, 5]`.
*   Swap `nums[0]` and `nums[1]`. The array becomes `[3, 9, 7, 5]`.
*   Swap `nums[1]` and `nums[2]`. The array becomes `[3, 7, 9, 5]`.
*   Swap `nums[2]` and `nums[3]`. The array becomes `[3, 7, 5, 9]`.
*   This array is good because it can be split into `[3]`, `[7, 5]`, and `[9]`.

**Example 3:**

**Input:** nums = [3,7,5,9], a = 4, b = 8

**Output:** 0

**Explanation:**

The array is already good. No swaps are needed.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= a < b <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int minAdjacentSwaps(int[] nums, int a, int b) {
        long ans = 0;
        long c1 = 0;
        long c2 = 0;
        for (int v : nums) {
            if (v < a) {
                ans += c1 + c2;
            } else if (v <= b) {
                c1++;
                ans += c2;
            } else {
                c2++;
            }
        }
        return (int) (ans % 1000000007);
    }
}
```