[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4017\. Peaks in Array II

Hard

You are given an integer array `nums` of length `n` and a 2D integer array `queries`.

A **non-empty subarrays** `nums[i..j]` is called a **peak subarray** if:

*   Its length is **at least** 3.
*   There exists an index `k` such that `i < k < j` and:
    *   `nums[k] > nums[k - 1]`
    *   `nums[k] > nums[k + 1]`

You have to process queries of two types:

*   <code>[1, l<sub>i</sub>, r<sub>i</sub>]</code>: Calculate the number of **peak subarrays** fully contained within <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>.
*   <code>[2, index<sub>i</sub>, val<sub>i</sub>]</code>: Update <code>nums[index<sub>i</sub>]</code> to <code>val<sub>i</sub></code>. This update applies to all subsequent queries.

Return an array `answer`, where `answer[i]` is the answer to the <code>i<sup>th</sup></code> query of type 1 in the order they appear.

**Example 1:**

**Input:** nums = [1,3,2,4], queries = \[\[1,0,3],[2,1,1],[1,0,3]]

**Output:** [2,0]

**Explanation:**

*   Query `[1, 0, 3]`:
    *   `[1, 3, 2]`: choose `k = 1`. Then `nums[k] = 3`, `nums[k - 1] = 1`, and `nums[k + 1] = 2`. Since `3 > 1` and `3 > 2`, this is a peak subarray.
    *   `[1, 3, 2, 4]`: choose `k = 1`. Then `nums[k] = 3`, `nums[k - 1] = 1`, and `nums[k + 1] = 2`. Since `3 > 1` and `3 > 2`, this is a peak subarray.
*   Query `[2, 1, 1]`: Update `nums[1]` to 1. The array becomes `[1, 1, 2, 4]`.
*   Query `[1, 0, 3]`: There are no peak subarrays now.
*   Thus, `answer = [2, 0]`.

**Example 2:**

**Input:** nums = [9,8,9,8], queries = \[\[1,1,3],[2,2,1],[1,0,2]]

**Output:** [1,0]

**Explanation:**

*   Query `[1, 1, 3]`:
    *   `nums[1..3] = [8, 9, 8]`: choose `k = 2`. Then `nums[k] = 9`, `nums[k - 1] = 8`, and `nums[k + 1] = 8`. Since `9 > 8` and `9 > 8`, this is a peak subarray.
*   Query `[2, 2, 1]`: Update `nums[2]` to 1. The array becomes `[9, 8, 1, 8]`.
*   Query `[1, 0, 2]`: There are no peak subarrays.
*   Thus, `answer = [1, 0]`.

**Example 3:**

**Input:** nums = [3,6,2,7,1], queries = \[\[1,1,3],[2,3,0],[1,0,4]]

**Output:** [0,3]

**Explanation:**

*   Query `[1, 1, 3]`: The only subarray of length at least 3 is `[6, 2, 7]`. Its only possible peak index is `k = 2`, but `nums[2] = 2` is less than both `nums[1] = 6` and `nums[3] = 7`, so it is not a peak subarray.
*   Query `[2, 3, 0]`: Update `nums[3]` to 0. The array becomes `[3, 6, 2, 0, 1]`.
*   Query `[1, 0, 4]`:
    *   `[3, 6, 2]`: choose `k = 1`. Then `nums[k] = 6`, `nums[k - 1] = 3`, and `nums[k + 1] = 2`. Since `6 > 3` and `6 > 2`, this is a peak subarray.
    *   `[3, 6, 2, 0]`: choose `k = 1`. Then `nums[k] = 6`, `nums[k - 1] = 3`, and `nums[k + 1] = 2`. Since `6 > 3` and `6 > 2`, this is a peak subarray.
    *   `[3, 6, 2, 0, 1]`: choose `k = 1`. Then `nums[k] = 6`, `nums[k - 1] = 3`, and `nums[k + 1] = 2`. Since `6 > 3` and `6 > 2`, this is a peak subarray.
*   Thus, `answer = [0, 3]`.

**Constraints:**

*   <code>3 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>queries[i] = [1, l<sub>i</sub>, r<sub>i</sub>]</code> or <code>queries[i] = [2, index<sub>i</sub>, val<sub>i</sub>]</code>
*   <code>0 <= l<sub>i</sub> < r<sub>i</sub> <= n - 1</code>
*   <code>0 <= index<sub>i</sub> <= n - 1</code>
*   <code>0 <= val<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    private int[] cnt;
    private int[] first;
    private int[] last;
    private long[] inner;
    private int qCnt;
    private int qFirst;
    private int qLast;
    private long qInner;

    public long[] countOfPeaks(int[] nums, int[][] queries) {
        int n = nums.length;
        int type1Count = 0;
        for (int[] q : queries) {
            if (q[0] == 1) {
                type1Count++;
            }
        }
        long[] ans = new long[type1Count];
        if (n < 3) {
            return ans;
        }
        int m = n - 2;
        cnt = new int[4 * m];
        first = new int[4 * m];
        last = new int[4 * m];
        inner = new long[4 * m];
        build(1, 1, m, nums);
        int ansIdx = 0;
        for (int[] q : queries) {
            if (q[0] == 1) {
                int l = q[1];
                int r = q[2];
                if (r - l + 1 < 3) {
                    ans[ansIdx++] = 0L;
                } else {
                    qCnt = 0;
                    qFirst = -1;
                    qLast = -1;
                    qInner = 0L;
                    query(1, 1, m, l + 1, r - 1);
                    if (qCnt == 0) {
                        ans[ansIdx++] = 0L;
                    } else {
                        long totalSub = tri((long) r - l + 1);
                        long leftLen = (long) qFirst - l + 1;
                        long rightLen = (long) r - qLast + 1;
                        long nonPeak = (tri(leftLen) - 1L) + qInner + tri(rightLen);
                        ans[ansIdx++] = totalSub - nonPeak;
                    }
                }
            } else {
                int idx = q[1];
                nums[idx] = q[2];
                int start = Math.max(1, idx - 1);
                int end = Math.min(m, idx + 1);
                for (int pos = start; pos <= end; pos++) {
                    update(1, 1, m, pos, isPeak(nums, pos));
                }
            }
        }
        return ans;
    }

    private boolean isPeak(int[] nums, int i) {
        return nums[i] > nums[i - 1] && nums[i] > nums[i + 1];
    }

    private long tri(long len) {
        return len * (len + 1L) / 2L;
    }

    private void build(int node, int tl, int tr, int[] nums) {
        if (tl == tr) {
            if (isPeak(nums, tl)) {
                cnt[node] = 1;
                first[node] = tl;
                last[node] = tl;
            } else {
                first[node] = -1;
                last[node] = -1;
            }
            return;
        }
        int mid = (tl + tr) >>> 1;
        build(node << 1, tl, mid, nums);
        build(node << 1 | 1, mid + 1, tr, nums);
        pull(node);
    }

    private void update(int node, int tl, int tr, int pos, boolean peak) {
        if (tl == tr) {
            if (peak) {
                cnt[node] = 1;
                first[node] = tl;
                last[node] = tl;
            } else {
                cnt[node] = 0;
                first[node] = -1;
                last[node] = -1;
            }
            inner[node] = 0L;
            return;
        }
        int mid = (tl + tr) >>> 1;
        if (pos <= mid) {
            update(node << 1, tl, mid, pos, peak);
        } else {
            update(node << 1 | 1, mid + 1, tr, pos, peak);
        }
        pull(node);
    }

    private void pull(int node) {
        int lc = node << 1;
        int rc = node << 1 | 1;
        if (cnt[lc] == 0) {
            cnt[node] = cnt[rc];
            first[node] = first[rc];
            last[node] = last[rc];
            inner[node] = inner[rc];
        } else if (cnt[rc] == 0) {
            cnt[node] = cnt[lc];
            first[node] = first[lc];
            last[node] = last[lc];
            inner[node] = inner[lc];
        } else {
            cnt[node] = cnt[lc] + cnt[rc];
            first[node] = first[lc];
            last[node] = last[rc];
            long gap = (long) first[rc] - last[lc] + 1L;
            inner[node] = inner[lc] + inner[rc] + tri(gap) - 1L;
        }
    }

    private void query(int node, int tl, int tr, int ql, int qr) {
        if (ql <= tl && tr <= qr) {
            if (cnt[node] == 0) {
                return;
            }
            if (qCnt == 0) {
                qCnt = cnt[node];
                qFirst = first[node];
                qLast = last[node];
                qInner = inner[node];
            } else {
                long gap = (long) first[node] - qLast + 1L;
                qInner += inner[node] + tri(gap) - 1L;
                qCnt += cnt[node];
                qLast = last[node];
            }
            return;
        }
        int mid = (tl + tr) >>> 1;
        if (ql <= mid) {
            query(node << 1, tl, mid, ql, qr);
        }
        if (qr > mid) {
            query(node << 1 | 1, mid + 1, tr, ql, qr);
        }
    }
}
```