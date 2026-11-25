[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4033\. Valid K-Unique Subarrays I

Hard

You are given an integer array `nums` and an integer `k`.

You are also given a 2D integer array `queries`, where <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> represents the non-empty subarrays <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>.

For each query, the **subarray** <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code> is considered **valid** if:

*   It contains **exactly** `k` **distinct** numbers, and
*   The **frequency** of every number in the **subarray** is **even**.

Return a boolean array `ans`, where `ans[i]` is `true` if <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code> is **valid**, and `false` otherwise.

**Example 1:**

**Input:** nums = [1,2,2,1], k = 2, queries = \[\[0,1],[0,3],[1,2]]

**Output:** [false,true,false]

**Explanation:**

| `i` | <code>[l<sub>i</sub>, r<sub>i</sub>]</code> | Subarray | Unique numbers | Frequency | Validity check |
| --- | --- | --- | --- | --- | --- |
| 0 | [0, 1] | [1, 2] | {1, 2} → 2 | {1: 1, 2: 1} | `false`: Element counts are not even. |
| 1 | [0, 3] | [1, 2, 2, 1] | {1, 2} → 2 | {1: 2, 2: 2} | `true`: Exactly `k = 2` distinct elements, all appear an even number of times. |
| 2 | [1, 2] | [2, 2] | {2} → 1 | {2: 2} | `false`: Number of distinct elements is less than `k = 2`. |

Thus, `ans = [false, true, false]`.

**Example 2:**

**Input:** nums = [3,3,3], k = 1, queries = \[\[1,2],[0,2]]

**Output:** [true,false]

**Explanation:**

| `i` | <code>[l<sub>i</sub>, r<sub>i</sub>]</code> | Subarray | Unique numbers | Frequency | Validity check |
| --- | --- | --- | --- | --- | --- |
| 0 | [1, 2] | [3, 3] | {3} → 1 | {3: 2} | `true`: Exactly `k = 1` distinct element, appears an even number of times. |
| 1 | [0, 2] | [3, 3, 3] | {3} → 1 | {3: 3} | `false`: 3 does not appear an even number of times. |

Thus, `ans = [true, false]`.

**Constraints:**

*   <code>2 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= k <= n`
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>queries[i] == [l<sub>i</sub>, r<sub>i</sub>]</code>
*   <code>0 <= l<sub>i</sub> < r<sub>i</sub> <= n - 1</code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int countOdd;
    private int countDistinct;

    public boolean[] validSubarrays(int[] nums, int k, int[][] queries) {
        int maxNum = Integer.MIN_VALUE;
        int[] prefixXor = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            maxNum = Math.max(maxNum, nums[i]);
            prefixXor[i + 1] = prefixXor[i] ^ nums[i];
        }
        int[][] sortedQueries = new int[queries.length][];
        int queriesCount = this.sortQueries(nums, k, prefixXor, queries, sortedQueries);
        boolean[] result = new boolean[queries.length];
        if (queriesCount == 0) {
            return result;
        }
        int currentLeft = sortedQueries[0][0];
        int currentRight = sortedQueries[0][0] - 1;
        countOdd = 0;
        countDistinct = 0;
        int[] counts = new int[maxNum + 1];
        for (int i = 0; i < queriesCount; i++) {
            int[] query = sortedQueries[i];
            while (currentLeft > query[0]) {
                this.add(counts, nums, --currentLeft);
            }
            while (currentRight < query[1]) {
                this.add(counts, nums, ++currentRight);
            }
            while (currentLeft < query[0]) {
                this.remove(counts, nums, currentLeft++);
            }
            while (currentRight > query[1]) {
                this.remove(counts, nums, currentRight--);
            }
            result[query[2]] = countDistinct == k && countOdd == 0;
        }
        return result;
    }

    private int sortQueries(
            int[] nums, int k, int[] prefixXor, int[][] queries, int[][] sortedQueries) {
        int blockSize = Math.max(1, (int) (nums.length / Math.sqrt(queries.length)));
        int queriesCount = 0;
        for (int i = 0; i < queries.length; i++) {
            int[] query = queries[i];
            int length = query[1] - query[0];
            if (length % 2 == 1
                    && length + 1 >= 2 * k
                    && (prefixXor[query[1] + 1] ^ prefixXor[query[0]]) == 0) {
                sortedQueries[queriesCount++] =
                        new int[] {query[0], query[1], i, query[0] / blockSize};
            }
        }
        Arrays.sort(
                sortedQueries,
                0,
                queriesCount,
                (o1, o2) -> {
                    if (o1[3] == o2[3]) {
                        return o1[3] % 2 == 0 ? o1[1] - o2[1] : o2[1] - o1[1];
                    } else {
                        return Integer.compare(o1[3], o2[3]);
                    }
                });
        return queriesCount;
    }

    private void add(int[] counts, int[] nums, int index) {
        int count = ++counts[nums[index]];
        if (count == 1) {
            countDistinct++;
        }
        countOdd += 2 * (count & 1) - 1;
    }

    private void remove(int[] counts, int[] nums, int index) {
        int count = --counts[nums[index]];
        if (count == 0) {
            countDistinct--;
        }
        countOdd += 2 * (count & 1) - 1;
    }
}
```