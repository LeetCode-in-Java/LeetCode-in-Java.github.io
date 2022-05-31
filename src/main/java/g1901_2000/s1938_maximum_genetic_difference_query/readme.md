## 1938\. Maximum Genetic Difference Query

Hard

There is a rooted tree consisting of `n` nodes numbered `0` to `n - 1`. Each node's number denotes its **unique genetic value** (i.e. the genetic value of node `x` is `x`). The **genetic difference** between two genetic values is defined as the **bitwise-XOR** of their values. You are given the integer array `parents`, where `parents[i]` is the parent for node `i`. If node `x` is the **root** of the tree, then `parents[x] == -1`.

You are also given the array `queries` where <code>queries[i] = [node<sub>i</sub>, val<sub>i</sub>]</code>. For each query `i`, find the **maximum genetic difference** between <code>val<sub>i</sub></code> and <code>p<sub>i</sub></code>, where <code>p<sub>i</sub></code> is the genetic value of any node that is on the path between <code>node<sub>i</sub></code> and the root (including <code>node<sub>i</sub></code> and the root). More formally, you want to maximize <code>val<sub>i</sub> XOR p<sub>i</sub></code>.

Return _an array_ `ans` _where_ `ans[i]` _is the answer to the_ <code>i<sup>th</sup></code> _query_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/29/c1.png)

**Input:** parents = [-1,0,1,1], queries = [[0,2],[3,2],[2,5]]

**Output:** [2,3,7]

**Explanation:** The queries are processed as follows: 

- [0,2]: The node with the maximum genetic difference is 0, with a difference of 2 XOR 0 = 2. 

- [3,2]: The node with the maximum genetic difference is 1, with a difference of 2 XOR 1 = 3. 

- [2,5]: The node with the maximum genetic difference is 2, with a difference of 5 XOR 2 = 7.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/29/c2.png)

**Input:** parents = [3,7,-1,2,0,7,0,2], queries = [[4,6],[1,15],[0,5]]

**Output:** [6,14,7]

**Explanation:** The queries are processed as follows: 

- [4,6]: The node with the maximum genetic difference is 0, with a difference of 6 XOR 0 = 6. 

- [1,15]: The node with the maximum genetic difference is 1, with a difference of 15 XOR 1 = 14. 

- [0,5]: The node with the maximum genetic difference is 2, with a difference of 5 XOR 2 = 7.

**Constraints:**

*   <code>2 <= parents.length <= 10<sup>5</sup></code>
*   `0 <= parents[i] <= parents.length - 1` for every node `i` that is **not** the root.
*   `parents[root] == -1`
*   <code>1 <= queries.length <= 3 * 10<sup>4</sup></code>
*   <code>0 <= node<sub>i</sub> <= parents.length - 1</code>
*   <code>0 <= val<sub>i</sub> <= 2 * 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int[] maxGeneticDifference(int[] parents, int[][] queries) {
        int n = parents.length;
        int[][] fd = new int[n][];
        for (int i = 0; i < n; i++) {
            fill(parents, n, fd, i);
        }
        int[] ret = new int[queries.length];
        for (int q = 0; q < queries.length; q++) {
            int cur = queries[q][0];
            int value = queries[q][1];
            for (int p = 30; p >= 0; p--) {
                int msk = 1 << p;
                if ((value & msk) != (cur & msk)) {
                    ret[q] |= msk;
                } else if (fd[cur][p] >= 0) {
                    ret[q] |= msk;
                    cur = fd[cur][p];
                }
            }
        }
        return ret;
    }

    private void fill(int[] parents, int n, int[][] fd, int i) {
        if (fd[i] == null) {
            fd[i] = new int[31];
            int a = parents[i];
            if (a >= 0) {
                fill(parents, n, fd, a);
            }
            for (int p = 30; p >= 0; p--) {
                if (a == -1) {
                    fd[i][p] = -1;
                } else {
                    if ((i & (1 << p)) == (a & (1 << p))) {
                        fd[i][p] = fd[a][p];
                    } else {
                        fd[i][p] = a;
                        a = fd[a][p];
                    }
                }
            }
        }
    }
}
```