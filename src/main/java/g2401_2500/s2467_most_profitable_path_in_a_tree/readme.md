[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2467\. Most Profitable Path in a Tree

Medium

There is an undirected tree with `n` nodes labeled from `0` to `n - 1`, rooted at node `0`. You are given a 2D integer array `edges` of length `n - 1` where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the tree.

At every node `i`, there is a gate. You are also given an array of even integers `amount`, where `amount[i]` represents:

*   the price needed to open the gate at node `i`, if `amount[i]` is negative, or,
*   the cash reward obtained on opening the gate at node `i`, otherwise.

The game goes on as follows:

*   Initially, Alice is at node `0` and Bob is at node `bob`.
*   At every second, Alice and Bob **each** move to an adjacent node. Alice moves towards some **leaf node**, while Bob moves towards node `0`.
*   For **every** node along their path, Alice and Bob either spend money to open the gate at that node, or accept the reward. Note that:
    *   If the gate is **already open**, no price will be required, nor will there be any cash reward.
    *   If Alice and Bob reach the node **simultaneously**, they share the price/reward for opening the gate there. In other words, if the price to open the gate is `c`, then both Alice and Bob pay `c / 2` each. Similarly, if the reward at the gate is `c`, both of them receive `c / 2` each.
*   If Alice reaches a leaf node, she stops moving. Similarly, if Bob reaches node `0`, he stops moving. Note that these events are **independent** of each other.

Return _the **maximum** net income Alice can have if she travels towards the optimal leaf node._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/10/29/eg1.png)

**Input:** edges = \[\[0,1],[1,2],[1,3],[3,4]], bob = 3, amount = [-2,4,2,-4,6]

**Output:** 6

**Explanation:**

The above diagram represents the given tree. The game goes as follows:

- Alice is initially on node 0, Bob on node 3. They open the gates of their respective nodes.

Alice's net income is now -2.

- Both Alice and Bob move to node 1.

Since they reach here simultaneously, they open the gate together and share the reward.

Alice's net income becomes -2 + (4 / 2) = 0.

- Alice moves on to node 3. Since Bob already opened its gate, Alice's income remains unchanged.

Bob moves on to node 0, and stops moving.

- Alice moves on to node 4 and opens the gate there. Her net income becomes 0 + 6 = 6.

Now, neither Alice nor Bob can make any further moves, and the game ends.

It is not possible for Alice to get a higher net income. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/10/29/eg2.png)

**Input:** edges = \[\[0,1]], bob = 1, amount = [-7280,2350]

**Output:** -7280

**Explanation:**

Alice follows the path 0->1 whereas Bob follows the path 1->0.

Thus, Alice opens the gate at node 0 only. Hence, her net income is -7280. 

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> < n</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   `edges` represents a valid tree.
*   `1 <= bob < n`
*   `amount.length == n`
*   `amount[i]` is an **even** integer in the range <code>[-10<sup>4</sup>, 10<sup>4</sup>]</code>.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int mostProfitablePath(int[][] edges, int bob, int[] amount) {
        int n = amount.length;
        int[][] g = packU(n, edges);
        int[][] pars = parents(g, 0);
        int[] par = pars[0];
        int[] ord = pars[1];
        int[] dep = pars[2];
        int u = bob;
        for (int i = 0; i < (dep[bob] + 1) / 2; i++) {
            amount[u] = 0;
            u = par[u];
        }
        if (dep[bob] % 2 == 0) {
            amount[u] /= 2;
        }
        int[] dp = new int[n];
        for (int i = n - 1; i >= 0; i--) {
            int cur = ord[i];
            if (g[cur].length == 1 && i > 0) {
                dp[cur] = amount[cur];
            } else {
                dp[cur] = Integer.MIN_VALUE / 2;
                for (int e : g[cur]) {
                    if (par[cur] == e) {
                        continue;
                    }
                    dp[cur] = Math.max(dp[cur], dp[e] + amount[cur]);
                }
            }
        }
        return dp[0];
    }

    private int[][] parents(int[][] g, int root) {
        int n = g.length;
        int[] par = new int[n];
        Arrays.fill(par, -1);
        int[] depth = new int[n];
        depth[0] = 0;
        int[] q = new int[n];
        q[0] = root;
        int r = 1;
        for (int p = 0; p < r; p++) {
            int cur = q[p];
            for (int nex : g[cur]) {
                if (par[cur] != nex) {
                    q[r++] = nex;
                    par[nex] = cur;
                    depth[nex] = depth[cur] + 1;
                }
            }
        }
        return new int[][] {par, q, depth};
    }

    private int[][] packU(int n, int[][] ft) {
        int[][] g = new int[n][];
        int[] p = new int[n];
        for (int[] u : ft) {
            p[u[0]]++;
            p[u[1]]++;
        }
        for (int i = 0; i < n; i++) {
            g[i] = new int[p[i]];
        }
        for (int[] u : ft) {
            g[u[0]][--p[u[0]]] = u[1];
            g[u[1]][--p[u[1]]] = u[0];
        }
        return g;
    }
}
```