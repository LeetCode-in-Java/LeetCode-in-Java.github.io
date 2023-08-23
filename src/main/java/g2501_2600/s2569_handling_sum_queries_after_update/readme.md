[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2569\. Handling Sum Queries After Update

Hard

You are given two **0-indexed** arrays `nums1` and `nums2` and a 2D array `queries` of queries. There are three types of queries:

1.  For a query of type 1, `queries[i] = [1, l, r]`. Flip the values from `0` to `1` and from `1` to `0` in `nums1` from index `l` to index `r`. Both `l` and `r` are **0-indexed**.
2.  For a query of type 2, `queries[i] = [2, p, 0]`. For every index `0 <= i < n`, set `nums2[i] = nums2[i] + nums1[i] * p`.
3.  For a query of type 3, `queries[i] = [3, 0, 0]`. Find the sum of the elements in `nums2`.

Return _an array containing all the answers to the third type queries._

**Example 1:**

**Input:** nums1 = [1,0,1], nums2 = [0,0,0], queries = \[\[1,1,1],[2,1,0],[3,0,0]]

**Output:** [3]

**Explanation:** After the first query nums1 becomes [1,1,1]. After the second query, nums2 becomes [1,1,1], so the answer to the third query is 3. Thus, [3] is returned.

**Example 2:**

**Input:** nums1 = [1], nums2 = [5], queries = \[\[2,0,0],[3,0,0]]

**Output:** [5]

**Explanation:** After the first query, nums2 remains [5], so the answer to the second query is 5. Thus, [5] is returned.

**Constraints:**

*   <code>1 <= nums1.length,nums2.length <= 10<sup>5</sup></code>
*   `nums1.length = nums2.length`
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   `queries[i].length = 3`
*   `0 <= l <= r <= nums1.length - 1`
*   <code>0 <= p <= 10<sup>6</sup></code>
*   `0 <= nums1[i] <= 1`
*   <code>0 <= nums2[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Deque;
import java.util.LinkedList;

public class Solution {
    public long[] handleQuery(int[] nums1, int[] nums2, int[][] queries) {
        Deque<Long> dq = new LinkedList<>();
        long sum = 0;
        for (int i : nums2) {
            sum += i;
        }
        Segment root = build(nums1, 0, nums1.length - 1);
        for (int[] q : queries) {
            if (1 == q[0]) {
                root.flip(q[1], q[2]);
            } else if (2 == q[0]) {
                sum += root.sum * q[1];
            } else {
                dq.add(sum);
            }
        }
        int n = dq.size();
        int i = 0;
        long[] res = new long[n];
        while (!dq.isEmpty()) {
            res[i++] = dq.poll();
        }
        return res;
    }

    private static class Segment {
        long sum;
        int f;
        int lo;
        int hi;
        Segment left;
        Segment right;

        public Segment(int l, int r) {
            lo = l;
            hi = r;
        }

        public void flip(int l, int r) {
            if (hi < l || r < lo) {
                return;
            }
            if (l <= lo && hi <= r) {
                f ^= 1;
                sum = hi - lo + 1 - sum;
                return;
            }
            if (1 == f) {
                left.flip(lo, hi);
                right.flip(lo, hi);
                f ^= 1;
            }
            left.flip(l, r);
            right.flip(l, r);
            sum = left.sum + right.sum;
        }
    }

    private Segment build(int[] nums, int l, int r) {
        if (l == r) {
            Segment node = new Segment(l, r);
            node.sum = nums[l];
            return node;
        }
        int mid = l + ((r - l) >> 1);
        Segment left = build(nums, l, mid);
        Segment right = build(nums, mid + 1, r);
        Segment root = new Segment(l, r);
        root.left = left;
        root.right = right;
        root.sum = left.sum + right.sum;
        return root;
    }
}
```