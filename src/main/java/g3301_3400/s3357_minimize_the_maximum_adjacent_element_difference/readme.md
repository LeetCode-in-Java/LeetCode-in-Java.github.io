[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3357\. Minimize the Maximum Adjacent Element Difference

Hard

You are given an array of integers `nums`. Some values in `nums` are **missing** and are denoted by -1.

You can choose a pair of **positive** integers `(x, y)` **exactly once** and replace each **missing** element with _either_ `x` or `y`.

You need to **minimize** the **maximum** **absolute difference** between _adjacent_ elements of `nums` after replacements.

Return the **minimum** possible difference.

**Example 1:**

**Input:** nums = [1,2,-1,10,8]

**Output:** 4

**Explanation:**

By choosing the pair as `(6, 7)`, nums can be changed to `[1, 2, 6, 10, 8]`.

The absolute differences between adjacent elements are:

*   `|1 - 2| == 1`
*   `|2 - 6| == 4`
*   `|6 - 10| == 4`
*   `|10 - 8| == 2`

**Example 2:**

**Input:** nums = [-1,-1,-1]

**Output:** 0

**Explanation:**

By choosing the pair as `(4, 4)`, nums can be changed to `[4, 4, 4]`.

**Example 3:**

**Input:** nums = [-1,10,-1,8]

**Output:** 1

**Explanation:**

By choosing the pair as `(11, 9)`, nums can be changed to `[11, 10, 9, 8]`.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either -1 or in the range <code>[1, 10<sup>9</sup>]</code>.

## Solution

```java
public class Solution {
    public int minDifference(int[] nums) {
        int n = nums.length;
        int maxAdj = 0;
        int mina = Integer.MAX_VALUE;
        int maxb = Integer.MIN_VALUE;
        for (int i = 0; i < n - 1; ++i) {
            int a = nums[i];
            int b = nums[i + 1];
            if (a > 0 && b > 0) {
                maxAdj = Math.max(maxAdj, Math.abs(a - b));
            } else if (a > 0 || b > 0) {
                mina = Math.min(mina, Math.max(a, b));
                maxb = Math.max(maxb, Math.max(a, b));
            }
        }
        int res = 0;
        for (int i = 0; i < n; ++i) {
            if ((i > 0 && nums[i - 1] == -1) || nums[i] > 0) {
                continue;
            }
            int j = i;
            while (j < n && nums[j] == -1) {
                j++;
            }
            int a = Integer.MAX_VALUE;
            int b = Integer.MIN_VALUE;
            if (i > 0) {
                a = Math.min(a, nums[i - 1]);
                b = Math.max(b, nums[i - 1]);
            }
            if (j < n) {
                a = Math.min(a, nums[j]);
                b = Math.max(b, nums[j]);
            }
            if (a <= b) {
                if (j - i == 1) {
                    res = Math.max(res, Math.min(maxb - a, b - mina));
                } else {
                    res =
                            Math.max(
                                    res,
                                    Math.min(
                                            maxb - a,
                                            Math.min(b - mina, (maxb - mina + 2) / 3 * 2)));
                }
            }
        }
        return Math.max(maxAdj, (res + 1) / 2);
    }
}
```