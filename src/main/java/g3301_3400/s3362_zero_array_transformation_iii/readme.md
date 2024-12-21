[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3362\. Zero Array Transformation III

Medium

You are given an integer array `nums` of length `n` and a 2D array `queries` where <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>.

Each `queries[i]` represents the following action on `nums`:

*   Decrement the value at each index in the range <code>[l<sub>i</sub>, r<sub>i</sub>]</code> in `nums` by **at most** 1.
*   The amount by which the value is decremented can be chosen **independently** for each index.

A **Zero Array** is an array with all its elements equal to 0.

Return the **maximum** number of elements that can be removed from `queries`, such that `nums` can still be converted to a **zero array** using the _remaining_ queries. If it is not possible to convert `nums` to a **zero array**, return -1.

**Example 1:**

**Input:** nums = [2,0,2], queries = \[\[0,2],[0,2],[1,1]]

**Output:** 1

**Explanation:**

After removing `queries[2]`, `nums` can still be converted to a zero array.

*   Using `queries[0]`, decrement `nums[0]` and `nums[2]` by 1 and `nums[1]` by 0.
*   Using `queries[1]`, decrement `nums[0]` and `nums[2]` by 1 and `nums[1]` by 0.

**Example 2:**

**Input:** nums = [1,1,1,1], queries = \[\[1,3],[0,2],[1,3],[1,2]]

**Output:** 2

**Explanation:**

We can remove `queries[2]` and `queries[3]`.

**Example 3:**

**Input:** nums = [1,2,3,4], queries = \[\[0,3]]

**Output:** \-1

**Explanation:**

`nums` cannot be converted to a zero array even after using all the queries.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 2`
*   <code>0 <= l<sub>i</sub> <= r<sub>i</sub> < nums.length</code>

## Solution

```java
import java.util.Arrays;
import java.util.PriorityQueue;

public class Solution {
    public int maxRemoval(int[] nums, int[][] queries) {
        Arrays.sort(queries, (a, b) -> a[0] - b[0]);
        PriorityQueue<Integer> last = new PriorityQueue<>((a, b) -> b - a);
        int[] diffs = new int[nums.length + 1];
        int idx = 0;
        int cur = 0;
        for (int i = 0; i < nums.length; i++) {
            while (idx < queries.length && queries[idx][0] == i) {
                last.add(queries[idx][1]);
                idx++;
            }
            cur += diffs[i];
            while (cur < nums[i] && !last.isEmpty() && last.peek() >= i) {
                cur++;
                diffs[last.poll() + 1]--;
            }
            if (cur < nums[i]) {
                return -1;
            }
        }
        return last.size();
    }
}
```