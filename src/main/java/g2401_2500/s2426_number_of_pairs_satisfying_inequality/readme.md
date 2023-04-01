[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2426\. Number of Pairs Satisfying Inequality

Hard

You are given two **0-indexed** integer arrays `nums1` and `nums2`, each of size `n`, and an integer `diff`. Find the number of **pairs** `(i, j)` such that:

*   `0 <= i < j <= n - 1` **and**
*   `nums1[i] - nums1[j] <= nums2[i] - nums2[j] + diff`.

Return _the **number of pairs** that satisfy the conditions._

**Example 1:**

**Input:** nums1 = [3,2,5], nums2 = [2,2,1], diff = 1

**Output:** 3

**Explanation:**

There are 3 pairs that satisfy the conditions:

1. i = 0, j = 1: 3 - 2 <= 2 - 2 + 1. Since i < j and 1 <= 1, this pair satisfies the conditions.

2. i = 0, j = 2: 3 - 5 <= 2 - 1 + 1. Since i < j and -2 <= 2, this pair satisfies the conditions.

3. i = 1, j = 2: 2 - 5 <= 2 - 1 + 1. Since i < j and -3 <= 2, this pair satisfies the conditions.

Therefore, we return 3. 

**Example 2:**

**Input:** nums1 = [3,-1], nums2 = [-2,2], diff = -1

**Output:** 0

**Explanation:**

Since there does not exist any pair that satisfies the conditions, we return 0. 

**Constraints:**

*   `n == nums1.length == nums2.length`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= nums1[i], nums2[i] <= 10<sup>4</sup></code>
*   <code>-10<sup>4</sup> <= diff <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    private long[] cnt;

    private long find(int x, int n) {
        long res = 0;
        x = Math.min(x, n);
        while (x > 0) {
            res += cnt[x];
            x -= (x & (-x));
        }
        return res;
    }

    private void update(int x, int n) {
        while (x <= n) {
            cnt[x] += 1;
            x += (x & (-x));
        }
    }

    public long numberOfPairs(int[] nums1, int[] nums2, int diff) {
        int n = nums1.length;
        int[] nums = new int[n];
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < n; i++) {
            nums[i] = nums1[i] - nums2[i];
            min = Math.min(min, nums[i]);
            max = Math.max(max, nums[i]);
        }
        max = max - min + 2;
        long ans = 0;
        cnt = new long[50000];
        for (int i = 0; i < n; i++) {
            nums[i] = nums[i] - min + 1;
            ans += find(nums[i] + diff, max);
            update(nums[i], max);
        }
        return ans;
    }
}
```