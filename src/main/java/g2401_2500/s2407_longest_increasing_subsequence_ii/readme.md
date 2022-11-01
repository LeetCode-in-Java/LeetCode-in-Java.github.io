[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2407\. Longest Increasing Subsequence II

Hard

You are given an integer array `nums` and an integer `k`.

Find the longest subsequence of `nums` that meets the following requirements:

*   The subsequence is **strictly increasing** and
*   The difference between adjacent elements in the subsequence is **at most** `k`.

Return _the length of the **longest** **subsequence** that meets the requirements._

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [4,2,1,4,3,4,5,8,15], k = 3

**Output:** 5

**Explanation:**

The longest subsequence that meets the requirements is [1,3,4,5,8].

The subsequence has a length of 5, so we return 5.

Note that the subsequence [1,3,4,5,8,15] does not meet the requirements because 15 - 8 = 7 is larger than 3. 

**Example 2:**

**Input:** nums = [7,4,5,1,8,12,4,7], k = 5

**Output:** 4

**Explanation:**

The longest subsequence that meets the requirements is [4,5,8,12].

The subsequence has a length of 4, so we return 4. 

**Example 3:**

**Input:** nums = [1,5], k = 1

**Output:** 1

**Explanation:** The longest subsequence that meets the requirements is [1]. The subsequence has a length of 1, so we return 1. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], k <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    private static class SegTree {
        private int[] arr;
        private int n;

        public SegTree(int n) {
            this.n = n;
            arr = new int[2 * n];
        }

        public int query(int l, int r) {
            l += n;
            r += n;
            int ans = 0;
            while (l < r) {
                if ((l & 1) == 1) {
                    ans = Math.max(ans, arr[l]);
                    l += 1;
                }
                if ((r & 1) == 1) {
                    r -= 1;
                    ans = Math.max(ans, arr[r]);
                }
                l = l >> 1;
                r = r >> 1;
            }
            return ans;
        }

        public void update(int i, int val) {
            i += n;
            arr[i] = val;
            while (i > 0) {
                i = i >> 1;
                arr[i] = Math.max(arr[2 * i], arr[2 * i + 1]);
            }
        }
    }

    public int lengthOfLIS(int[] nums, int k) {
        int max = 0;
        for (int n : nums) {
            max = Math.max(max, n);
        }
        SegTree seg = new SegTree(max);
        int ans = 0;
        for (int n : nums) {
            n -= 1;
            int temp = seg.query(Math.max(0, n - k), n) + 1;
            ans = Math.max(ans, temp);
            seg.update(n, temp);
        }
        return ans;
    }
}
```