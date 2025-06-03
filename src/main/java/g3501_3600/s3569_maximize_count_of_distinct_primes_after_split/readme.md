[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3569\. Maximize Count of Distinct Primes After Split

Hard

You are given an integer array `nums` having length `n` and a 2D integer array `queries` where `queries[i] = [idx, val]`.

For each query:

1.  Update `nums[idx] = val`.
2.  Choose an integer `k` with `1 <= k < n` to split the array into the non-empty prefix `nums[0..k-1]` and suffix `nums[k..n-1]` such that the sum of the counts of **distinct** prime values in each part is **maximum**.

**Note:** The changes made to the array in one query persist into the next query.

Return an array containing the result for each query, in the order they are given.

**Example 1:**

**Input:** nums = [2,1,3,1,2], queries = \[\[1,2],[3,3]]

**Output:** [3,4]

**Explanation:**

*   Initially `nums = [2, 1, 3, 1, 2]`.
*   After 1<sup>st</sup> query, `nums = [2, 2, 3, 1, 2]`. Split `nums` into `[2]` and `[2, 3, 1, 2]`. `[2]` consists of 1 distinct prime and `[2, 3, 1, 2]` consists of 2 distinct primes. Hence, the answer for this query is `1 + 2 = 3`.
*   After 2<sup>nd</sup> query, `nums = [2, 2, 3, 3, 2]`. Split `nums` into `[2, 2, 3]` and `[3, 2]` with an answer of `2 + 2 = 4`.
*   The output is `[3, 4]`.

**Example 2:**

**Input:** nums = [2,1,4], queries = \[\[0,1]]

**Output:** [0]

**Explanation:**

*   Initially `nums = [2, 1, 4]`.
*   After 1<sup>st</sup> query, `nums = [1, 1, 4]`. There are no prime numbers in `nums`, hence the answer for this query is 0.
*   The output is `[0]`.

**Constraints:**

*   <code>2 <= n == nums.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `0 <= queries[i][0] < nums.length`
*   <code>1 <= queries[i][1] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;
import java.util.TreeSet;

@SuppressWarnings("java:S6541")
public class Solution {
    private static final int MAX_VAL = 100005;
    private static boolean[] isPrime = new boolean[MAX_VAL];

    static {
        Arrays.fill(isPrime, true);
        isPrime[0] = isPrime[1] = false;
        for (int i = 2; i * i < MAX_VAL; i++) {
            if (isPrime[i]) {
                for (int j = i * i; j < MAX_VAL; j += i) {
                    isPrime[j] = false;
                }
            }
        }
    }

    private static class Node {
        int maxVal;
        int lazyDelta;
    }

    private static class SegmentTree {
        Node[] tree;
        int n;

        public SegmentTree(int n) {
            this.n = n;
            tree = new Node[4 * this.n];
            for (int i = 0; i < 4 * this.n; i++) {
                tree[i] = new Node();
            }
        }

        private void push(int nodeIdx) {
            if (tree[nodeIdx].lazyDelta != 0) {
                tree[2 * nodeIdx].maxVal += tree[nodeIdx].lazyDelta;
                tree[2 * nodeIdx].lazyDelta += tree[nodeIdx].lazyDelta;
                tree[2 * nodeIdx + 1].maxVal += tree[nodeIdx].lazyDelta;
                tree[2 * nodeIdx + 1].lazyDelta += tree[nodeIdx].lazyDelta;
                tree[nodeIdx].lazyDelta = 0;
            }
        }

        private void update(int queryStart, int queryEnd, int delta) {
            queryStart = Math.max(1, queryStart);
            queryEnd = Math.min(n - 1, queryEnd);
            if (queryStart > queryEnd) {
                return;
            }
            update(1, 1, n - 1, queryStart, queryEnd, delta);
        }

        private void update(
                int nodeIdx, int start, int end, int queryStart, int queryEnd, int delta) {
            if (start > end || start > queryEnd || end < queryStart) {
                return;
            }
            if (queryStart <= start && end <= queryEnd) {
                tree[nodeIdx].maxVal += delta;
                tree[nodeIdx].lazyDelta += delta;
                return;
            }
            push(nodeIdx);

            int mid = (start + end) / 2;
            update(2 * nodeIdx, start, mid, queryStart, queryEnd, delta);
            update(2 * nodeIdx + 1, mid + 1, end, queryStart, queryEnd, delta);
            tree[nodeIdx].maxVal = Math.max(tree[2 * nodeIdx].maxVal, tree[2 * nodeIdx + 1].maxVal);
        }

        public int queryMax() {
            if (n - 1 < 1) {
                return 0;
            }
            return tree[1].maxVal;
        }
    }

    public int[] maximumCount(int[] nums, int[][] queries) {
        int n = nums.length;
        Map<Integer, TreeSet<Integer>> primeIndices = new HashMap<>();
        for (int i = 0; i < n; i++) {
            if (isPrime[nums[i]]) {
                primeIndices.computeIfAbsent(nums[i], k -> new TreeSet<>()).add(i);
            }
        }
        SegmentTree segmentTree = new SegmentTree(n);
        for (Map.Entry<Integer, TreeSet<Integer>> entry : primeIndices.entrySet()) {
            TreeSet<Integer> indices = entry.getValue();
            int first = indices.first();
            int last = indices.last();
            segmentTree.update(first + 1, last, 1);
        }
        int[] result = new int[queries.length];
        for (int q = 0; q < queries.length; q++) {
            int idx = queries[q][0];
            int val = queries[q][1];
            int oldVal = nums[idx];
            if (isPrime[oldVal]) {
                TreeSet<Integer> indices = primeIndices.get(oldVal);
                int oldFirst = indices.first();
                int oldLast = indices.last();
                indices.remove(idx);
                if (indices.isEmpty()) {
                    primeIndices.remove(oldVal);
                    segmentTree.update(oldFirst + 1, oldLast, -1);
                } else {
                    int newFirst = indices.first();
                    int newLast = indices.last();

                    if (idx == oldFirst && newFirst != oldFirst) {
                        segmentTree.update(oldFirst + 1, newFirst, -1);
                    }
                    if (idx == oldLast && newLast != oldLast) {
                        segmentTree.update(newLast + 1, oldLast, -1);
                    }
                }
            }
            nums[idx] = val;
            if (isPrime[val]) {
                boolean wasNewPrime = !primeIndices.containsKey(val);
                TreeSet<Integer> indices = primeIndices.computeIfAbsent(val, k -> new TreeSet<>());
                int oldFirst = indices.isEmpty() ? -1 : indices.first();
                int oldLast = indices.isEmpty() ? -1 : indices.last();
                indices.add(idx);
                int newFirst = indices.first();
                int newLast = indices.last();
                if (wasNewPrime) {
                    segmentTree.update(newFirst + 1, newLast, 1);
                } else {
                    if (idx < oldFirst) {
                        segmentTree.update(newFirst + 1, oldFirst, 1);
                    }
                    if (idx > oldLast) {
                        segmentTree.update(oldLast + 1, newLast, 1);
                    }
                }
            }
            int totalDistinctPrimesInCurrentNums = primeIndices.size();
            int maxIntersection = segmentTree.queryMax();
            maxIntersection = Math.max(0, maxIntersection);
            result[q] = totalDistinctPrimesInCurrentNums + maxIntersection;
        }
        return result;
    }
}
```