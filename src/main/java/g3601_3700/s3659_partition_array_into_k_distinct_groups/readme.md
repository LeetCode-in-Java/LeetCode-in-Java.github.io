[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3659\. Partition Array Into K-Distinct Groups

Medium

You are given an integer array `nums` and an integer `k`.

Your task is to determine whether it is possible to partition all elements of `nums` into one or more groups such that:

*   Each group contains **exactly** `k` **distinct** elements.
*   Each element in `nums` must be assigned to **exactly** one group.

Return `true` if such a partition is possible, otherwise return `false`.

**Example 1:**

**Input:** nums = [1,2,3,4], k = 2

**Output:** true

**Explanation:**

One possible partition is to have 2 groups:

*   Group 1: `[1, 2]`
*   Group 2: `[3, 4]`

Each group contains `k = 2` distinct elements, and all elements are used exactly once.

**Example 2:**

**Input:** nums = [3,5,2,2], k = 2

**Output:** true

**Explanation:**

One possible partition is to have 2 groups:

*   Group 1: `[2, 3]`
*   Group 2: `[2, 5]`

Each group contains `k = 2` distinct elements, and all elements are used exactly once.

**Example 3:**

**Input:** nums = [1,5,2,3], k = 3

**Output:** false

**Explanation:**

We cannot form groups of `k = 3` distinct elements using all values exactly once.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= k <= nums.length`

## Solution

```java
public class Solution {
    public boolean partitionArray(int[] nums, int k) {
        int n = nums.length;
        if (n % k != 0) {
            return false;
        }
        int max = 0;
        for (int x : nums) {
            max = Math.max(max, x);
        }
        int[] count = new int[max + 1];
        int limit = n / k;
        for (int x : nums) {
            if (++count[x] > limit) {
                return false;
            }
        }
        return true;
    }
}
```