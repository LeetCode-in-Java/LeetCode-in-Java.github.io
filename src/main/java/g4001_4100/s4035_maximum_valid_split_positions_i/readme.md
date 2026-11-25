[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4035\. Maximum Valid Split Positions I

Medium

You are given an integer array `nums`.

You may remove **at most one** element from `nums`. Let `arr` be the array of remaining elements in their original order, and let `m` be its length.

A **split position** `i` of `arr` is **valid** if:

*   `0 <= i < m - 1`, and
*   `gcd(arr[0..i]) == gcd(arr[i + 1..m - 1])`.

An array of length 1 has no valid split positions.

The **score** of `arr` is the number of valid split positions in it.

Return the **maximum possible score** of `arr`.

Here, `gcd(a)` denotes the **greatest common divisor** of all elements in the array `a`.

**Example 1:**

**Input:** nums = [10,30,15,10]

**Output:** 2

**Explanation:**

One optimal solution is to remove `nums[2] = 15`. Then `arr = [10, 30, 10]`.

The split positions are:

| Split Position `i` | `gcd(arr[0..i])` | `gcd(arr[i + 1..m - 1])` |
| --- | --- | --- |
| 0 | 10 | 10 |
| 1 | 10 | 10 |

All split positions are valid. Thus, the answer is 2.

**Example 2:**

**Input:** nums = [2,10,14]

**Output:** 1

**Explanation:**

One optimal solution is to not remove any element. Then `arr = [2, 10, 14]`.

The split positions are:

| Split Position `i` | `gcd(arr[0..i])` | `gcd(arr[i + 1..m - 1])` |
| --- | --- | --- |
| 0 | 2 | 2 |
| 1 | 2 | 14 |

Only the split position at index 0 is valid. Thus, the answer is 1.

**Example 3:**

**Input:** nums = [2,4]

**Output:** 0

**Explanation:**

The only remaining array that has a split position is `arr = [2, 4]`.

The split positions are:

| Split Position `i` | `gcd(arr[0..i])` | `gcd(arr[i + 1..m - 1])` |
| --- | --- | --- |
| 0 | 2 | 4 |

There are no valid split positions. Thus, the answer is 0.

**Constraints:**

*   `2 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int maxValidSplits(int[] nums) {
        int n = nums.length;
        int[] pref = new int[n + 1];
        int[] suf = new int[n + 1];
        for (int k = 0; k < n; k++) {
            pref[k + 1] = gcd(pref[k], nums[k]);
        }
        for (int k = n - 1; k >= 0; k--) {
            suf[k] = gcd(suf[k + 1], nums[k]);
        }
        int total = pref[n];
        int best = 0;
        for (int k = 0; k <= n; k++) {
            if (pref[k] == suf[k]) {
                best++;
            }
        }
        for (int i = 0; i < n; i++) {
            int g = gcd(pref[i], suf[i + 1]);
            if (g == total) {
                continue;
            }
            int l = pref[i] == g ? walk(nums, 0, 0, 1, g) : walk(nums, i + 1, pref[i], 1, g);
            int r =
                    suf[i + 1] == g
                            ? walk(nums, n - 1, 0, -1, g)
                            : walk(nums, i - 1, suf[i + 1], -1, g);
            best = Math.max(best, r - l + 2 - (l <= i && i <= r ? 1 : 0));
        }
        return best;
    }

    private int walk(int[] nums, int k, int cur, int step, int g) {
        while (cur != g) {
            cur = gcd(cur, nums[k]);
            k += step;
        }
        return k;
    }

    private int gcd(int a, int b) {
        while (b != 0) {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```