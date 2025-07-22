[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3624\. Number of Integers With Popcount-Depth Equal to K II

Hard

You are given an integer array `nums`.

For any positive integer `x`, define the following sequence:

*   <code>p<sub>0</sub> = x</code>
*   <code>p<sub>i+1</sub> = popcount(p<sub>i</sub>)</code> for all `i >= 0`, where `popcount(y)` is the number of set bits (1's) in the binary representation of `y`.

This sequence will eventually reach the value 1.

The **popcount-depth** of `x` is defined as the **smallest** integer `d >= 0` such that <code>p<sub>d</sub> = 1</code>.

For example, if `x = 7` (binary representation `"111"`). Then, the sequence is: `7 → 3 → 2 → 1`, so the popcount-depth of 7 is 3.

You are also given a 2D integer array `queries`, where each `queries[i]` is either:

*   `[1, l, r, k]` - **Determine** the number of indices `j` such that `l <= j <= r` and the **popcount-depth** of `nums[j]` is equal to `k`.
*   `[2, idx, val]` - **Update** `nums[idx]` to `val`.

Return an integer array `answer`, where `answer[i]` is the number of indices for the <code>i<sup>th</sup></code> query of type `[1, l, r, k]`.

**Example 1:**

**Input:** nums = [2,4], queries = \[\[1,0,1,1],[2,1,1],[1,0,1,0]]

**Output:** [2,1]

**Explanation:**

| `i` | `queries[i]` | `nums`   | binary(`nums`) | popcount-<br>depth  | `[l, r]` | `k` | Valid<br>`nums[j]`  | updated<br>`nums`  | Answer  |
|-----|--------------|----------|----------------|---------------------|----------|-----|---------------------|--------------------|---------|
| 0   | [1,0,1,1]    | [2,4]    | [10, 100]      | [1, 1]              | [0, 1]   | 1   | [0, 1]              | —                  | 2       |
| 1   | [2,1,1]      | [2,4]    | [10, 100]      | [1, 1]              | —        | —   | —                   | [2,1]              | —       |
| 2   | [1,0,1,0]    | [2,1]    | [10, 1]        | [1, 0]              | [0, 1]   | 0   | [1]                 | —                  | 1       |

Thus, the final `answer` is `[2, 1]`.

**Example 2:**

**Input:** nums = [3,5,6], queries = \[\[1,0,2,2],[2,1,4],[1,1,2,1],[1,0,1,0]]

**Output:** [3,1,0]

**Explanation:**

| `i` | `queries[i]`   | `nums`         | binary(`nums`)       | popcount-<br>depth | `[l, r]` | `k` | Valid<br>`nums[j]` | updated<br>`nums` | Answer |
|-----|----------------|----------------|-----------------------|---------------------|----------|-----|---------------------|--------------------|---------|
| 0   | [1,0,2,2]      | [3, 5, 6]      | [11, 101, 110]        | [2, 2, 2]           | [0, 2]   | 2   | [0, 1, 2]          | —                  | 3       |
| 1   | [2,1,4]        | [3, 5, 6]      | [11, 101, 110]        | [2, 2, 2]           | —        | —   | —                   | [3, 4, 6]          | —       |
| 2   | [1,1,2,1]      | [3, 4, 6]      | [11, 100, 110]        | [2, 1, 2]           | [1, 2]   | 1   | [1]                | —                  | 1       |
| 3   | [1,0,1,0]      | [3, 4, 6]      | [11, 100, 110]        | [2, 1, 2]           | [0, 1]   | 0   | []                 | —                  | 0       |

Thus, the final `answer` is `[3, 1, 0]`.

**Example 3:**

**Input:** nums = [1,2], queries = \[\[1,0,1,1],[2,0,3],[1,0,0,1],[1,0,0,2]]

**Output:** [1,0,1]

**Explanation:**

| `i` | `queries[i]`   | `nums`     | binary(`nums`) | popcount-<br>depth  | `[l, r]` | `k` | Valid<br>`nums[j]` | updated<br>`nums`  | Answer  |
|-----|----------------|------------|----------------|---------------------|----------|-----|--------------------|--------------------|---------|
| 0   | [1,0,1,1]      | [1, 2]     | [1, 10]        | [0, 1]              | [0, 1]   | 1   | [1]                | —                  | 1       |
| 1   | [2,0,3]        | [1, 2]     | [1, 10]        | [0, 1]              | —        | —   | —                  | [3, 2]             |         |
| 2   | [1,0,0,1]      | [3, 2]     | [11, 10]       | [2, 1]              | [0, 0]   | 1   | []                 | —                  | 0       |
| 3   | [1,0,0,2]      | [3, 2]     | [11, 10]       | [2, 1]              | [0, 0]   | 2   | [0]                | —                  | 1       |

Thus, the final `answer` is `[1, 0, 1]`.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>15</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 3` or `4`
    *   `queries[i] == [1, l, r, k]` or,
    *   `queries[i] == [2, idx, val]`
    *   `0 <= l <= r <= n - 1`
    *   `0 <= k <= 5`
    *   `0 <= idx <= n - 1`
    *   <code>1 <= val <= 10<sup>15</sup></code>

## Solution

```java
import java.util.ArrayList;

public class Solution {
    private static final int[] DEPTH_TABLE = new int[65];

    static {
        DEPTH_TABLE[1] = 0;
        for (int i = 2; i <= 64; ++i) {
            DEPTH_TABLE[i] = 1 + DEPTH_TABLE[Integer.bitCount(i)];
        }
    }

    private int computeDepth(long number) {
        if (number == 1) {
            return 0;
        }
        return 1 + DEPTH_TABLE[Long.bitCount(number)];
    }

    public int[] popcountDepth(long[] nums, long[][] queries) {
        int len = nums.length;
        int maxDepth = 6;
        FenwickTree[] trees = new FenwickTree[maxDepth];
        for (int d = 0; d < maxDepth; ++d) {
            trees[d] = new FenwickTree();
            trees[d].build(len);
        }
        for (int i = 0; i < len; ++i) {
            int depth = computeDepth(nums[i]);
            if (depth < maxDepth) {
                trees[depth].update(i + 1, 1);
            }
        }
        ArrayList<Integer> ansList = new ArrayList<>();
        for (long[] query : queries) {
            int type = (int) query[0];
            if (type == 1) {
                int left = (int) query[1];
                int right = (int) query[2];
                int depth = (int) query[3];
                if (depth >= 0 && depth < maxDepth) {
                    ansList.add(trees[depth].queryRange(left + 1, right + 1));
                } else {
                    ansList.add(0);
                }
            } else if (type == 2) {
                int index = (int) query[1];
                long newVal = query[2];
                int oldDepth = computeDepth(nums[index]);
                if (oldDepth < maxDepth) {
                    trees[oldDepth].update(index + 1, -1);
                }
                nums[index] = newVal;
                int newDepth = computeDepth(newVal);
                if (newDepth < maxDepth) {
                    trees[newDepth].update(index + 1, 1);
                }
            }
        }
        int[] ansArray = new int[ansList.size()];
        for (int i = 0; i < ansList.size(); i++) {
            ansArray[i] = ansList.get(i);
        }
        return ansArray;
    }

    private static class FenwickTree {
        private int[] tree;
        private int size;

        public FenwickTree() {
            this.size = 0;
        }

        public void build(int n) {
            this.size = n;
            this.tree = new int[size + 1];
        }

        public void update(int index, int value) {
            while (index <= size) {
                tree[index] += value;
                index += index & (-index);
            }
        }

        public int query(int index) {
            int result = 0;
            while (index > 0) {
                result += tree[index];
                index -= index & (-index);
            }
            return result;
        }

        public int queryRange(int left, int right) {
            if (left > right) {
                return 0;
            }
            return query(right) - query(left - 1);
        }
    }
}
```