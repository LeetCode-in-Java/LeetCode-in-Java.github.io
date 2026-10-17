[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3943\. Number of Pairs After Increment

Hard

You are given two integer arrays `nums1` and `nums2`, and a 2D integer array `queries`.

Each `queries[i]` is one of the following types:

*   `[1, x, y, val]` – **Add** `val` to every element in `nums2[x..y]`.
*   `[2, tot]` – **Compute** the number of pairs `(j, k)` such that `nums1[j] + nums2[k] == tot`.

Return an integer array `answer`, where `answer[j]` is the number of pairs for the <code>j<sup>th</sup></code> query of type 2.

**Example 1:**

**Input:** nums1 = [1,2], nums2 = [3,4], queries = \[\[2,5],[1,0,0,2],[2,5]]

**Output:** [2,1]

**Explanation:**

*   `queries[0] = [2, 5]`: Valid pairs are `nums1[0] + nums2[1] = 1 + 4 = 5` and `nums1[1] + nums2[0] = 2 + 3 = 5`.
*   `queries[1] = [1, 0, 0, 2]`: Add 2 to `nums2[0]`, resulting in `nums2 = [5, 4]`.
*   `queries[2] = [2, 5]`: Valid pair is `nums1[0] + nums2[1] = 1 + 4 = 5`.
*   Thus, the `answer = [2, 1]`.

**Example 2:**

**Input:** nums1 = [1,1], nums2 = [2,2,3], queries = \[\[2,4],[1,0,1,1],[2,4]]

**Output:** [2,6]

**Explanation:**

*   `queries[0] = [2, 4]`: Valid pairs are `nums1[0] + nums2[2] = 1 + 3` and `nums1[1] + nums2[2] = 1 + 3`.
*   `queries[1] = [1, 0, 1, 1]`: Add 1 to `nums2[0]` and `nums2[1]`, resulting in `nums2 = [3, 3, 3]`.
*   `queries[2] = [2, 4]`: Every element of `nums1 = [1, 1]` pairs with every element of `nums2 = [3, 3, 3]` as `1 + 3 = 4`. That gives `2 × 3 = 6` pairs in total.
*   Thus, the `answer = [2, 6]`.

**Example 3:**

**Input:** nums1 = [2,5,8,4], nums2 = [1,3,8], queries = \[\[2,9],[1,1,2,1],[2,10]]

**Output:** [1,0]

**Explanation:**

*   `queries[0] = [2, 9]`: Only valid pair is `nums1[2] + nums2[0] = 8 + 1 = 9`.
*   `queries[1] = [1, 1, 2, 1]`: Add 1 to `nums2[1]` and `nums2[2]`, resulting in `nums2 = [1, 4, 9]`.
*   `queries[2] = [2, 10]`: No pair sums to `10`.
*   Thus, the `answer = [1, 0]`.

**Constraints:**

*   `1 <= nums1.length <= 5`
*   <code>1 <= nums2.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= nums1[i], nums2[i] <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   `queries[i].length == 2 or 4`
    *   `queries[i] == [1, x, y, val], or`
    *   `queries[i] == [2, tot]`
    *   `0 <= x <= y < nums2.length`
    *   <code>1 <= val <= 10<sup>5</sup></code>
    *   <code>1 <= tot <= 10<sup>9</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

public class Solution {
    private HashMap<Long, Long> cache;

    private void update(long[] nums2, int l, int r, long val) {
        if (r < l) {
            return;
        }
        for (int i = l; i <= r; i++) {
            long oldValue = nums2[i];
            long newValue = nums2[i] + val;
            cache.put(oldValue, cache.getOrDefault(oldValue, 0L) - 1);
            cache.put(newValue, cache.getOrDefault(newValue, 0L) + 1);
            nums2[i] = newValue;
        }
    }

    public int[] numberOfPairs(int[] nums1, int[] nums2, int[][] queries) {
        long[] n2 = initialize(nums2);
        long globalOffset = 0;
        List<Integer> answers = new ArrayList<>();
        int q = 0;
        while (q < queries.length) {
            QueryResult result = processQuery(q, queries, nums1, n2, globalOffset);
            q = result.nextIndex;
            globalOffset = result.globalOffset;
            if (result.answer != null) {
                answers.add(result.answer);
            }
        }
        return toArray(answers);
    }

    private long[] initialize(int[] nums2) {
        cache = new HashMap<>();
        long[] result = new long[nums2.length];
        for (int i = 0; i < nums2.length; i++) {
            result[i] = nums2[i];
            incrementCount(result[i]);
        }
        return result;
    }

    private void incrementCount(long value) {
        cache.put(value, cache.getOrDefault(value, 0L) + 1);
    }

    private QueryResult processQuery(
            int index, int[][] queries, int[] nums1, long[] nums2, long globalOffset) {
        int[] query = queries[index];
        if (query[0] == 1) {
            return processUpdateQuery(index, queries, nums2, globalOffset);
        }
        int answer = countPairs(nums1, query[1], globalOffset);
        return new QueryResult(index + 1, globalOffset, answer);
    }

    private QueryResult processUpdateQuery(
            int index, int[][] queries, long[] nums2, long globalOffset) {
        int[] query = queries[index];
        int l = query[1];
        int r = query[2];
        long val = query[3];
        int nextIndex = index + 1;
        while (nextIndex < queries.length && hasSameRange(queries[nextIndex], l, r)) {
            val += queries[nextIndex][3];
            nextIndex++;
        }
        globalOffset = applyUpdate(nums2, l, r, val, globalOffset);
        return new QueryResult(nextIndex, globalOffset, null);
    }

    private boolean hasSameRange(int[] query, int l, int r) {
        return query.length == 4 && query[1] == l && query[2] == r;
    }

    private long applyUpdate(long[] nums2, int l, int r, long val, long globalOffset) {
        int length = r - l + 1;
        if (length <= nums2.length / 2) {
            update(nums2, l, r, val);
            return globalOffset;
        }
        update(nums2, 0, l - 1, -val);
        update(nums2, r + 1, nums2.length - 1, -val);
        return globalOffset + val;
    }

    private int countPairs(int[] nums1, long target, long globalOffset) {
        long totalPairs = 0;
        for (int num1 : nums1) {
            long num2 = target - num1 - globalOffset;
            totalPairs += cache.getOrDefault(num2, 0L);
        }
        return (int) totalPairs;
    }

    private int[] toArray(List<Integer> answers) {
        int[] result = new int[answers.size()];
        for (int i = 0; i < answers.size(); i++) {
            result[i] = answers.get(i);
        }
        return result;
    }

    private static final class QueryResult {
        private final int nextIndex;
        private final long globalOffset;
        private final Integer answer;

        private QueryResult(int nextIndex, long globalOffset, Integer answer) {
            this.nextIndex = nextIndex;
            this.globalOffset = globalOffset;
            this.answer = answer;
        }
    }
}
```