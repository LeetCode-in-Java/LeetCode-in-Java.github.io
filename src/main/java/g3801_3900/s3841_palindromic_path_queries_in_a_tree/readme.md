[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3841\. Palindromic Path Queries in a Tree

Hard

You are given an undirected tree with `n` nodes labeled 0 to `n - 1`. This is represented by a 2D array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates an undirected edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>.

You are also given a string `s` of length `n` consisting of lowercase English letters, where `s[i]` represents the character assigned to node `i`.

You are also given a string array `queries`, where each `queries[i]` is either:

*   <code>"update u<sub>i</sub> c"</code>: Change the character at node <code>u<sub>i</sub></code> to `c`. Formally, update <code>s[u<sub>i</sub>] = c</code>.
*   <code>"query u<sub>i</sub> v<sub>i</sub>"</code>: Determine whether the string formed by the characters on the **unique** path from <code>u<sub>i</sub></code> to <code>v<sub>i</sub></code> (inclusive) can be **rearranged** into a **palindrome**.

Return a boolean array `answer`, where `answer[j]` is `true` if the <code>j<sup>th</sup></code> query of type <code>"query u<sub>i</sub> v<sub>i</sub>"</code> can be rearranged into a **palindrome**, and `false` otherwise.

**Example 1:**

**Input:** n = 3, edges = \[\[0,1],[1,2]], s = "aac", queries = ["query 0 2","update 1 b","query 0 2"]

**Output:** [true,false]

**Explanation:**

*   `"query 0 2"`: Path `0 → 1 → 2` gives `"aac"`, which can be rearranged to form `"aca"`, a palindrome. Thus, `answer[0] = true`.
*   `"update 1 b"`: Update node 1 to `'b'`, now `s = "abc"`.
*   `"query 0 2"`: Path characters are `"abc"`, which cannot be rearranged to form a palindrome. Thus, `answer[1] = false`.

Thus, `answer = [true, false]`.

**Example 2:**

**Input:** n = 4, edges = \[\[0,1],[0,2],[0,3]], s = "abca", queries = ["query 1 2","update 0 b","query 2 3","update 3 a","query 1 3"]

**Output:** [false,false,true]

**Explanation:**

*   `"query 1 2"`: Path `1 → 0 → 2` gives `"bac"`, which cannot be rearranged to form a palindrome. Thus, `answer[0] = false`.
*   `"update 0 b"`: Update node 0 to `'b'`, now `s = "bbca"`.
*   `"query 2 3"`: Path `2 → 0 → 3` gives `"cba"`, which cannot be rearranged to form a palindrome. Thus, `answer[1] = false`.
*   `"update 3 a"`: Update node 3 to `'a'`, `s = "bbca"`.
*   `"query 1 3"`: Path `1 → 0 → 3` gives `"bba"`, which can be rearranged to form `"bab"`, a palindrome. Thus, `answer[2] = true`.

Thus, `answer = [false, false, true]`.

**Constraints:**

*   <code>1 <= n == s.length <= 5 * 10<sup>4</sup></code>
*   `edges.length == n - 1`
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   `s` consists of lowercase English letters.
*   The input is generated such that `edges` represents a valid tree.
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
    *   <code>queries[i] = "update u<sub>i</sub> c"</code> or
    *   <code>queries[i] = "query u<sub>i</sub> v<sub>i</sub>"</code>
    *   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
    *   `c` is a lowercase English letter.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    private int[] head;
    private int[] to;
    private int[] next;

    private int[][] lift;
    private int[] first;
    private int[] last;
    private int[] depth;
    private int time = 0;
    private int maxPower;

    public List<Boolean> palindromePath(int n, int[][] edges, String s, String[] queries) {
        this.first = new int[n];
        this.last = new int[n];
        this.maxPower = 31 - Integer.numberOfLeadingZeros(n);
        this.depth = new int[n];
        this.lift = new int[maxPower + 1][n];
        int m = edges.length;
        this.head = new int[n];
        this.to = new int[m << 1];
        this.next = new int[m << 1];
        Arrays.fill(head, -1);
        for (int i = 0; i < m; i++) {
            int a = edges[i][0];
            int b = edges[i][1];
            to[i << 1] = b;
            next[i << 1] = head[a];
            head[a] = i << 1;
            to[i << 1 | 1] = a;
            next[i << 1 | 1] = head[b];
            head[b] = i << 1 | 1;
        }
        dfs(0, -1);
        BIT bit = new BIT(n + 1);
        char[] arr = s.toCharArray();
        for (int i = 0; i < n; i++) {
            bit.update(first[i], 1 << arr[i] - 'a');
            bit.update(last[i] + 1, 1 << arr[i] - 'a');
        }
        ArrayList<Boolean> list = new ArrayList<>(queries.length);
        for (String query : queries) {
            if (query.charAt(0) == 'u') {
                int i = Integer.parseInt(query.substring(7, query.length() - 2));
                char c = query.charAt(query.length() - 1);
                if (c == arr[i]) {
                    continue;
                }
                int filter = 1 << arr[i] - 'a' | 1 << c - 'a';
                bit.update(first[i], filter);
                arr[i] = c;
                bit.update(last[i] + 1, filter);
            } else {
                int i1 = query.indexOf(' ');
                int i2 = query.lastIndexOf(' ');
                int a = Integer.parseInt(query.substring(i1 + 1, i2));
                int b = Integer.parseInt(query.substring(i2 + 1));
                int r = bit.query(first[a]) ^ bit.query(first[b]) ^ 1 << arr[lca(a, b)] - 'a';
                list.add((r & r - 1) == 0);
            }
        }
        return list;
    }

    private int lca(int a, int b) {
        if (depth[a] > depth[b]) {
            int temp = a;
            a = b;
            b = temp;
        }
        if (first[a] <= first[b] && last[a] >= last[b]) {
            return a;
        }
        int diff = depth[b] - depth[a];
        for (int i = maxPower; diff != 0; --i) {
            if (diff >= 1 << i) {
                b = lift[i][b];
                diff -= 1 << i;
            }
        }
        if (a == b) {
            return a;
        }
        for (int i = maxPower; i >= 0; --i) {
            if (lift[i][a] != lift[i][b]) {
                a = lift[i][a];
                b = lift[i][b];
            }
        }
        return lift[0][a];
    }

    private void dfs(int index, int prev) {
        first[index] = ++time;
        for (int x = head[index]; x != -1; x = next[x]) {
            int nextIndex = to[x];
            if (nextIndex == prev) {
                continue;
            }
            depth[nextIndex] = depth[index] + 1;
            lift[0][nextIndex] = index;
            for (int i = 1; (1 << i) < depth[nextIndex]; i++) {
                lift[i][nextIndex] = lift[i - 1][lift[i - 1][nextIndex]];
            }
            dfs(nextIndex, index);
        }
        last[index] = time;
    }

    private static class BIT {
        private final int[] ints;
        private final int n;

        BIT(int n) {
            this.n = n;
            this.ints = new int[n + 1];
        }

        public void update(int index, int val) {
            for (int i = index + 1; i <= n; i += i & -i) {
                ints[i] ^= val;
            }
        }

        public int query(int index) {
            int ans = 0;
            for (int i = index + 1; i > 0; i -= i & -i) {
                ans ^= ints[i];
            }
            return ans;
        }
    }
}
```