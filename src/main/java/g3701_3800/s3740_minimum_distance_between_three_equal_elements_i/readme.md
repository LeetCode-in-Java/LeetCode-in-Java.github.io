[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3740\. Minimum Distance Between Three Equal Elements I

Easy

You are given an integer array `nums`.

A tuple `(i, j, k)` of 3 **distinct** indices is **good** if `nums[i] == nums[j] == nums[k]`.

The **distance** of a **good** tuple is `abs(i - j) + abs(j - k) + abs(k - i)`, where `abs(x)` denotes the **absolute value** of `x`.

Return an integer denoting the **minimum** possible **distance** of a **good** tuple. If no **good** tuples exist, return `-1`.

**Example 1:**

**Input:** nums = [1,2,1,1,3]

**Output:** 6

**Explanation:**

The minimum distance is achieved by the good tuple `(0, 2, 3)`.

`(0, 2, 3)` is a good tuple because `nums[0] == nums[2] == nums[3] == 1`. Its distance is `abs(0 - 2) + abs(2 - 3) + abs(3 - 0) = 2 + 1 + 3 = 6`.

**Example 2:**

**Input:** nums = [1,1,2,3,2,1,2]

**Output:** 8

**Explanation:**

The minimum distance is achieved by the good tuple `(2, 4, 6)`.

`(2, 4, 6)` is a good tuple because `nums[2] == nums[4] == nums[6] == 2`. Its distance is `abs(2 - 4) + abs(4 - 6) + abs(6 - 2) = 2 + 2 + 4 = 8`.

**Example 3:**

**Input:** nums = [1]

**Output:** \-1

**Explanation:**

There are no good tuples. Therefore, the answer is -1.

**Constraints:**

*   `1 <= n == nums.length <= 100`
*   `1 <= nums[i] <= n`

## Solution

```java
public class Solution {
    public int minimumDistance(int[] nums) {
        int len = nums.length;
        int[] last2 = new int[len];
        int res = 200;
        for (int i = 0; i < len; i++) {
            int val = nums[i] - 1;
            int pos = i + 1;
            int pack = last2[val];
            int old = pack & 255;
            int cur = pack >> 8;
            last2[val] = cur | (pos << 8);
            if (old > 0) {
                res = Math.min(res, (pos - old) << 1);
            }
        }
        return res == 200 ? -1 : res;
    }
}
```