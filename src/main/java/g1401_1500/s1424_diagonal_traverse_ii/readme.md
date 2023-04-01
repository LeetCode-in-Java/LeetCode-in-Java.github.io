[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1424\. Diagonal Traverse II

Medium

Given a 2D integer array `nums`, return _all elements of_ `nums` _in diagonal order as shown in the below images_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/04/08/sample_1_1784.png)

**Input:** nums = \[\[1,2,3],[4,5,6],[7,8,9]]

**Output:** [1,4,2,7,5,3,8,6,9]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/04/08/sample_2_1784.png)

**Input:** nums = \[\[1,2,3,4,5],[6,7],[8],[9,10,11],[12,13,14,15,16]]

**Output:** [1,6,2,8,7,3,9,4,12,10,5,13,11,14,15,16]

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i].length <= 10<sup>5</sup></code>
*   <code>1 <= sum(nums[i].length) <= 10<sup>5</sup></code>
*   <code>1 <= nums[i][j] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.Objects;

public class Solution {
    public int[] findDiagonalOrder(List<List<Integer>> nums) {
        List<Integer> ans = new ArrayList<>();
        ArrayDeque<Iterator<Integer>> queue = new ArrayDeque<>();
        int pos = 0;
        do {
            if (pos < nums.size()) {
                queue.offerFirst(nums.get(pos).iterator());
            }
            int sz = queue.size();
            while (--sz >= 0) {
                Iterator<Integer> cur = queue.poll();
                ans.add(Objects.requireNonNull(cur).next());
                if (cur.hasNext()) {
                    queue.offer(cur);
                }
            }
            pos++;
        } while (!queue.isEmpty() || pos < nums.size());
        return ans.stream().mapToInt(o -> o).toArray();
    }
}
```