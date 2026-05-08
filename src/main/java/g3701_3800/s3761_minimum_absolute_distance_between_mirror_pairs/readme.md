[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3761\. Minimum Absolute Distance Between Mirror Pairs

Medium

You are given an integer array `nums`.

A **mirror pair** is a pair of indices `(i, j)` such that:

*   `0 <= i < j < nums.length`, and
*   `reverse(nums[i]) == nums[j]`, where `reverse(x)` denotes the integer formed by reversing the digits of `x`. Leading zeros are omitted after reversing, for example `reverse(120) = 21`.

Return the **minimum** absolute distance between the indices of any mirror pair. The absolute distance between indices `i` and `j` is `abs(i - j)`.

If no mirror pair exists, return `-1`.

**Example 1:**

**Input:** nums = [12,21,45,33,54]

**Output:** 1

**Explanation:**

The mirror pairs are:

*   (0, 1) since `reverse(nums[0]) = reverse(12) = 21 = nums[1]`, giving an absolute distance `abs(0 - 1) = 1`.
*   (2, 4) since `reverse(nums[2]) = reverse(45) = 54 = nums[4]`, giving an absolute distance `abs(2 - 4) = 2`.

The minimum absolute distance among all pairs is 1.

**Example 2:**

**Input:** nums = [120,21]

**Output:** 1

**Explanation:**

There is only one mirror pair (0, 1) since `reverse(nums[0]) = reverse(120) = 21 = nums[1]`.

The minimum absolute distance is 1.

**Example 3:**

**Input:** nums = [21,120]

**Output:** \-1

**Explanation:**

There are no mirror pairs in the array.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;

public class Solution {
    public int minMirrorPairDistance(int[] nums) {
        int res = 100000;
        int i = 0;
        HashMap<Integer, Integer> seen = new HashMap<>();
        for (int n : nums) {
            int r;
            if (seen.containsKey(n)) {
                res = Math.min(res, i - seen.get(n));
            }
            for (r = 0; n > 0; n /= 10) {
                r = r * 10 + (n % 10);
            }
            seen.put(r, i++);
        }

        return res == 100000 ? -1 : res;
    }
}
```