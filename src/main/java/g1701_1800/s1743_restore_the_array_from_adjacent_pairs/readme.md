## 1743\. Restore the Array From Adjacent Pairs

Medium

There is an integer array `nums` that consists of `n` **unique** elements, but you have forgotten it. However, you do remember every pair of adjacent elements in `nums`.

You are given a 2D integer array `adjacentPairs` of size `n - 1` where each <code>adjacentPairs[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates that the elements <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> are adjacent in `nums`.

It is guaranteed that every adjacent pair of elements `nums[i]` and `nums[i+1]` will exist in `adjacentPairs`, either as `[nums[i], nums[i+1]]` or `[nums[i+1], nums[i]]`. The pairs can appear **in any order**.

Return _the original array_ `nums`_. If there are multiple solutions, return **any of them**_.

**Example 1:**

**Input:** adjacentPairs = [[2,1],[3,4],[3,2]]

**Output:** [1,2,3,4]

**Explanation:** This array has all its adjacent pairs in adjacentPairs. Notice that adjacentPairs[i] may not be in left-to-right order.

**Example 2:**

**Input:** adjacentPairs = [[4,-2],[1,4],[-3,1]]

**Output:** [-2,4,1,-3]

**Explanation:** There can be negative numbers. Another solution is [-3,1,4,-2], which would also be accepted.

**Example 3:**

**Input:** adjacentPairs = [[100000,-100000]]

**Output:** [100000,-100000]

**Constraints:**

*   `nums.length == n`
*   `adjacentPairs.length == n - 1`
*   `adjacentPairs[i].length == 2`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= nums[i], u<sub>i</sub>, v<sub>i</sub> <= 10<sup>5</sup></code>
*   There exists some `nums` that has `adjacentPairs` as its pairs.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public int[] restoreArray(int[][] adjacentPairs) {
        if (adjacentPairs == null || adjacentPairs.length == 0) {
            return new int[0];
        }
        if (adjacentPairs.length == 1) {
            return adjacentPairs[0];
        }
        Map<Integer, List<Integer>> graph = new HashMap<>();
        for (int[] pair : adjacentPairs) {
            graph.computeIfAbsent(pair[0], k -> new ArrayList<>()).add(pair[1]);
            graph.computeIfAbsent(pair[1], k -> new ArrayList<>()).add(pair[0]);
        }
        int[] res = new int[graph.size()];
        for (Map.Entry<Integer, List<Integer>> entry : graph.entrySet()) {
            if (entry.getValue().size() == 1) {
                res[0] = entry.getKey();
                break;
            }
        }
        res[1] = graph.get(res[0]).get(0);
        for (int i = 2; i < res.length; i++) {
            for (int cur : graph.get(res[i - 1])) {
                if (cur != res[i - 2]) {
                    res[i] = cur;
                    break;
                }
            }
        }
        return res;
    }
}
```