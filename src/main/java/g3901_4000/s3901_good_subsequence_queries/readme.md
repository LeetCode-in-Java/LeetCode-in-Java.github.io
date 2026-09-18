[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3901\. Good Subsequence Queries

Hard

You are given an integer array `nums` of length `n` and an integer `p`.

A **non-empty subsequence** of `nums` is called **good** if:

*   Its length is **strictly less** than `n`.
*   The **greatest common divisor (GCD)** of its elements is **exactly** `p`.

You are also given a 2D integer array `queries` of length `q`, where each <code>queries[i] = [ind<sub>i</sub>, val<sub>i</sub>]</code> indicates that you should update <code>nums[ind<sub>i</sub>]</code> to <code>val<sub>i</sub></code>.

After each query, determine whether there exists **any good subsequence** in the current array.

Return the **number** of queries for which a **good subsequence** exists.

The term `gcd(a, b)` denotes the **greatest common divisor** of `a` and `b`.

**Example 1:**

**Input:** nums = [4,8,12,16], p = 2, queries = \[\[0,3],[2,6]]

**Output:** 1

**Explanation:**

| i | `[ind_i, val_i]` | Operation | Updated `nums` | Any good Subsequence |
|---:|---|---|---|---|
| 0 | `[0, 3]` | Update `nums[0]` to `3` | `[3, 8, 12, 16]` | No, as no subsequence has GCD exactly `p = 2` |
| 1 | `[2, 6]` | Update `nums[2]` to `6` | `[3, 8, 6, 16]` | Yes, subsequence `[8, 6]` has GCD exactly `p = 2` |

Thus, the answer is 1.

**Example 2:**

**Input:** nums = [4,5,7,8], p = 3, queries = \[\[0,6],[1,9],[2,3]]

**Output:** 2

**Explanation:**

| i | `[ind_i, val_i]` | Operation | Updated `nums` | Any good Subsequence |
|---:|---|---|---|---|
| 0 | `[0, 6]` | Update `nums[0]` to `6` | `[6, 5, 7, 8]` | No, as no subsequence has GCD exactly `p = 3` |
| 1 | `[1, 9]` | Update `nums[1]` to `9` | `[6, 9, 7, 8]` | Yes, subsequence `[6, 9]` has GCD exactly `p = 3` |
| 2 | `[2, 3]` | Update `nums[2]` to `3` | `[6, 9, 3, 8]` | Yes, subsequence `[6, 9, 3]` has GCD exactly `p = 3` |

Thus, the answer is 2.

**Example 3:**

**Input:** nums = [5,7,9], p = 2, queries = \[\[1,4],[2,8]]

**Output:** 0

**Explanation:**

| i | `[ind_i, val_i]` | Operation | Updated `nums` | Any good Subsequence |
|---:|---|---|---|---|
| 0 | `[1, 4]` | Update `nums[1]` to `4` | `[5, 4, 9]` | No, as no subsequence has GCD exactly `p = 2` |
| 1 | `[2, 8]` | Update `nums[2]` to `8` | `[5, 4, 8]` | No, as no subsequence has GCD exactly `p = 2` |

Thus, the answer is 0.

**Constraints:**

*   <code>2 <= n == nums.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 5 * 10<sup>4</sup></code>
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   <code>queries[i] = [ind<sub>i</sub>, val<sub>i</sub>]</code>
*   <code>1 <= val<sub>i</sub>, p <= 5 * 10<sup>4</sup></code>
*   <code>0 <= ind<sub>i</sub> <= n - 1</code>

## Solution

```java
public class Solution {
    private int[] tree;
    private int validCount = 0;

    public int countGoodSubseq(int[] nums, int p, int[][] queries) {
        int n = nums.length;
        // 1. Iterative Segment Tree only needs 2n space
        tree = new int[2 * n];
        // Build the leaves of the tree and get initial count
        for (int i = 0; i < n; i++) {
            if (nums[i] % p == 0) {
                tree[n + i] = nums[i];
                validCount++;
            }
        }
        // Build the internal nodes bottom-up
        for (int i = n - 1; i > 0; --i) {
            // tree[i << 1] is the left child, tree[i << 1 | 1] is the right child
            tree[i] = gcd(tree[i << 1], tree[i << 1 | 1]);
        }
        int ans = 0;
        for (int[] q : queries) {
            int idx = q[0];
            int value = q[1];
            // 2. O(1) tracking for validCount (No segment tree needed for this!)
            boolean wasValid = (nums[idx] % p == 0);
            boolean isValid = (value % p == 0);
            if (wasValid && !isValid) {
                validCount--;
            }
            if (!wasValid && isValid) {
                validCount++;
            }
            // Update the original array to keep track of the old values
            nums[idx] = value;
            // Point update for the Iterative Tree
            tree[idx + n] = isValid ? value : 0;
            // Climb up the tree using bitwise shifts (i >>= 1 means i = i / 2)
            for (int i = idx + n; i > 1; i >>= 1) {
                tree[i >> 1] = gcd(tree[i], tree[i ^ 1]);
            }
            // 3. The logic check (tree[1] is ALWAYS the root in an iterative tree)
            if (tree[1] == p) {
                if (validCount < n) {
                    ans++;
                } else {
                    // validCount == n (Every element is a multiple of p)
                    if (n > 20) {
                        // THE O(1) MATH BYPASS!
                        ans++;
                    } else {
                        // Only run this heavy check if n is 20 or smaller
                        boolean flag = false;
                        for (int i = 0; i < n; i++) {
                            int leftGcd = query(0, i - 1, n);
                            int rightGcd = query(i + 1, n - 1, n);
                            if (gcd(leftGcd, rightGcd) == p) {
                                flag = true;
                                break;
                            }
                        }
                        if (flag) {
                            ans++;
                        }
                    }
                }
            }
        }
        return ans;
    }

    // Iterative Range Query [l, r] inclusive
    private int query(int l, int r, int n) {
        if (l > r) {
            return 0;
        }
        int res = 0;
        for (l += n, r += n + 1; l < r; l >>= 1, r >>= 1) {
            if ((l & 1) == 1) {
                res = gcd(res, tree[l++]);
            }
            if ((r & 1) == 1) {
                res = gcd(res, tree[--r]);
            }
        }
        return res;
    }

    private int gcd(int a, int b) {
        while (b > 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }
}
```