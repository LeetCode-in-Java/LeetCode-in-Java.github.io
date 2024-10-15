[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2612\. Minimum Reverse Operations

Hard

You are given an integer `n` and an integer `p` in the range `[0, n - 1]`. Representing a **0-indexed** array `arr` of length `n` where all positions are set to `0`'s, except position `p` which is set to `1`.

You are also given an integer array `banned` containing some positions from the array. For the **i**<sup>**th**</sup> position in `banned`, `arr[banned[i]] = 0`, and `banned[i] != p`.

You can perform **multiple** operations on `arr`. In an operation, you can choose a **subarray** with size `k` and **reverse** the subarray. However, the `1` in `arr` should never go to any of the positions in `banned`. In other words, after each operation `arr[banned[i]]` **remains** `0`.

_Return an array_ `ans` _where_ _for each_ `i` _from_ `[0, n - 1]`, `ans[i]` _is the **minimum** number of reverse operations needed to bring the_ `1` _to position_ `i` _in arr_, _or_ `-1` _if it is impossible_.

*   A **subarray** is a contiguous **non-empty** sequence of elements within an array.
*   The values of `ans[i]` are independent for all `i`'s.
*   The **reverse** of an array is an array containing the values in **reverse order**.

**Example 1:**

**Input:** n = 4, p = 0, banned = [1,2], k = 4

**Output:** [0,-1,-1,1]

**Explanation:**

In this case `k = 4` so there is only one possible reverse operation we can perform, which is reversing the whole array. Initially, 1 is placed at position 0 so the amount of operations we need for position 0 is `0`. We can never place a 1 on the banned positions, so the answer for positions 1 and 2 is `-1`. Finally, with one reverse operation we can bring the 1 to index 3, so the answer for position 3 is `1`.

**Example 2:**

**Input:** n = 5, p = 0, banned = [2,4], k = 3

**Output:** [0,-1,-1,-1,-1]

**Explanation:**

In this case the 1 is initially at position 0, so the answer for that position is `0`. We can perform reverse operations of size 3. The 1 is currently located at position 0, so we need to reverse the subarray `[0, 2]` for it to leave that position, but reversing that subarray makes position 2 have a 1, which shouldn't happen. So, we can't move the 1 from position 0, making the result for all the other positions `-1`.

**Example 3:**

**Input:** n = 4, p = 2, banned = [0,1,3], k = 1

**Output:** [-1,-1,0,-1]

**Explanation:** In this case we can only perform reverse operations of size 1.So the 1 never changes its position.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `0 <= p <= n - 1`
*   `0 <= banned.length <= n - 1`
*   `0 <= banned[i] <= n - 1`
*   `1 <= k <= n`
*   `banned[i] != p`
*   all values in `banned` are **unique**

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public int[] minReverseOperations(int n, int p, int[] banned, int k) {
        int[] out = new int[n];
        Arrays.fill(out, -1);
        for (int node : banned) {
            out[node] = -2;
        }
        List<Integer> nodes = new ArrayList<>();
        nodes.add(p);
        int depth = 0;
        out[p] = depth;
        int step = k - 1;
        int[] nextNode2s = new int[n + 1];
        for (int i = 0; i < n + 1; i++) {
            nextNode2s[i] = i + 2;
        }
        while (!nodes.isEmpty()) {
            depth++;
            List<Integer> newNodes = new ArrayList<>();
            for (int node1 : nodes) {
                int loReverseStart = Math.max(node1 - step, 0);
                int hiReverseStart = Math.min(node1, n - k);
                int loNode2 = 2 * loReverseStart + k - 1 - node1;
                int hiNode2 = 2 * hiReverseStart + k - 1 - node1;
                int postHiNode2 = hiNode2 + 2;
                int node2 = loNode2;
                while (node2 <= hiNode2) {
                    int nextNode2 = nextNode2s[node2];
                    nextNode2s[node2] = postHiNode2;
                    if (node2 < n && out[node2] == -1) {
                        newNodes.add(node2);
                        out[node2] = depth;
                    }
                    node2 = nextNode2;
                }
            }
            nodes = newNodes;
        }
        for (int i = 0; i < n; i++) {
            if (out[i] == -2) {
                out[i] = -1;
            }
        }
        return out;
    }
}
```