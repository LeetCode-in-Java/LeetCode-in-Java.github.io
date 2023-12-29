[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2948\. Make Lexicographically Smallest Array by Swapping Elements

Medium

You are given a **0-indexed** array of **positive** integers `nums` and a **positive** integer `limit`.

In one operation, you can choose any two indices `i` and `j` and swap `nums[i]` and `nums[j]` **if** `|nums[i] - nums[j]| <= limit`.

Return _the **lexicographically smallest array** that can be obtained by performing the operation any number of times_.

An array `a` is lexicographically smaller than an array `b` if in the first position where `a` and `b` differ, array `a` has an element that is less than the corresponding element in `b`. For example, the array `[2,10,3]` is lexicographically smaller than the array `[10,2,3]` because they differ at index `0` and `2 < 10`.

**Example 1:**

**Input:** nums = [1,5,3,9,8], limit = 2

**Output:** [1,3,5,8,9]

**Explanation:** Apply the operation 2 times:

- Swap nums[1] with nums[2]. The array becomes [1,3,5,9,8]

- Swap nums[3] with nums[4]. The array becomes [1,3,5,8,9]

We cannot obtain a lexicographically smaller array by applying any more operations.

Note that it may be possible to get the same result by doing different operations.

**Example 2:**

**Input:** nums = [1,7,6,18,2,1], limit = 3

**Output:** [1,6,7,18,1,2]

**Explanation:** Apply the operation 3 times:

- Swap nums[1] with nums[2]. The array becomes [1,6,7,18,2,1]

- Swap nums[0] with nums[4]. The array becomes [2,6,7,18,1,1]

- Swap nums[0] with nums[5]. The array becomes [1,6,7,18,1,2]

We cannot obtain a lexicographically smaller array by applying any more operations.

**Example 3:**

**Input:** nums = [1,7,28,19,10], limit = 3

**Output:** [1,7,28,19,10]

**Explanation:** [1,7,28,19,10] is the lexicographically smallest array we can obtain because we cannot apply the operation on any two indices.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= limit <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int[] lexicographicallySmallestArray(int[] nums, int limit) {
        int n = nums.length;
        Node[] nodes = new Node[n];
        for (int i = 0; i < n; i++) {
            nodes[i] = new Node(i, nums[i]);
        }
        Arrays.sort(nodes, (a, b) -> Integer.signum(a.value - b.value));
        int group = 1;
        nodes[0].group = group;
        for (int i = 1; i < n; i++) {
            if (Math.abs(nodes[i].value - nodes[i - 1].value) <= limit) {
                nodes[i].group = group;
            } else {
                nodes[i].group = ++group;
            }
        }
        int[] groupBase = new int[group + 1];
        for (int i = n - 1; i >= 0; i--) {
            groupBase[nodes[i].group] = i;
        }
        int[] groupIndex = new int[n];
        for (Node node : nodes) {
            groupIndex[node.id] = node.group;
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; i++) {
            int index = groupBase[groupIndex[i]];
            ans[i] = nodes[index].value;
            groupBase[groupIndex[i]]++;
        }
        return ans;
    }

    private static class Node {
        int id;
        int value;
        int group;

        Node(int id, int value) {
            this.id = id;
            this.value = value;
        }
    }
}
```