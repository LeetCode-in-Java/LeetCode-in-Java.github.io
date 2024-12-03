[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1916\. Count Ways to Build Rooms in an Ant Colony

Hard

You are an ant tasked with adding `n` new rooms numbered `0` to `n-1` to your colony. You are given the expansion plan as a **0-indexed** integer array of length `n`, `prevRoom`, where `prevRoom[i]` indicates that you must build room `prevRoom[i]` before building room `i`, and these two rooms must be connected **directly**. Room `0` is already built, so `prevRoom[0] = -1`. The expansion plan is given such that once all the rooms are built, every room will be reachable from room `0`.

You can only build **one room** at a time, and you can travel freely between rooms you have **already built** only if they are **connected**. You can choose to build **any room** as long as its **previous room** is already built.

Return _the **number of different orders** you can build all the rooms in_. Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/19/d1.JPG)

**Input:** prevRoom = [-1,0,1]

**Output:** 1

**Explanation:** There is only one way to build the additional rooms: 0 → 1 → 2

**Example 2:**

**![](https://assets.leetcode.com/uploads/2021/06/19/d2.JPG)**

**Input:** prevRoom = [-1,0,0,1,2]

**Output:** 6

**Explanation:** The 6 ways are: 

0 → 1 → 3 → 2 → 4 

0 → 2 → 4 → 1 → 3 

0 → 1 → 2 → 3 → 4 

0 → 1 → 2 → 4 → 3 

0 → 2 → 1 → 3 → 4 

0 → 2 → 1 → 4 → 3

**Constraints:**

*   `n == prevRoom.length`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   `prevRoom[0] == -1`
*   `0 <= prevRoom[i] < n` for all `1 <= i < n`
*   Every room is reachable from room `0` once all the rooms are built.

## Solution

```java
import java.math.BigInteger;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    private static final int MOD = 1000000007;
    private List<Integer>[] graph;
    private long[] fact;

    public int waysToBuildRooms(int[] prevRoom) {
        int n = prevRoom.length;
        graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        fact = new long[prevRoom.length + 10];
        fact[0] = fact[1] = 1;
        for (int i = 2; i < fact.length; i++) {
            fact[i] = fact[i - 1] * i;
            fact[i] %= MOD;
        }
        for (int i = 1; i < prevRoom.length; i++) {
            int pre = prevRoom[i];
            graph[pre].add(i);
        }

        long[] res = dfs(0);
        return (int) (res[1] % MOD);
    }

    private long[] dfs(int root) {
        long[] res = new long[] {1, 0};
        int cnt = 0;
        List<long[]> list = new ArrayList<>();
        for (int next : graph[root]) {
            long[] v = dfs(next);
            cnt += (int) v[0];
            list.add(v);
        }
        res[0] += cnt;
        long com = 1;
        for (long[] p : list) {
            long choose = c(cnt, (int) (p[0]));
            cnt -= (int) p[0];
            com = com * choose;
            com %= MOD;
            com = com * p[1];
            com %= MOD;
        }
        res[1] = com;
        return res;
    }

    private long c(int i, int j) {
        long mod = 1000000007;
        long prevRoom = fact[i];
        long b = ((fact[i - j] % mod) * (fact[j] % mod)) % mod;
        BigInteger value = BigInteger.valueOf(b);
        long binverse = value.modInverse(BigInteger.valueOf(mod)).longValue();
        return (prevRoom * (binverse % mod)) % mod;
    }
}
```