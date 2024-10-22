[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3327\. Check if DFS Strings Are Palindromes

Hard

You are given a tree rooted at node 0, consisting of `n` nodes numbered from `0` to `n - 1`. The tree is represented by an array `parent` of size `n`, where `parent[i]` is the parent of node `i`. Since node 0 is the root, `parent[0] == -1`.

You are also given a string `s` of length `n`, where `s[i]` is the character assigned to node `i`.

Consider an empty string `dfsStr`, and define a recursive function `dfs(int x)` that takes a node `x` as a parameter and performs the following steps in order:

*   Iterate over each child `y` of `x` **in increasing order of their numbers**, and call `dfs(y)`.
*   Add the character `s[x]` to the end of the string `dfsStr`.

**Note** that `dfsStr` is shared across all recursive calls of `dfs`.

You need to find a boolean array `answer` of size `n`, where for each index `i` from `0` to `n - 1`, you do the following:

*   Empty the string `dfsStr` and call `dfs(i)`.
*   If the resulting string `dfsStr` is a **palindrome**, then set `answer[i]` to `true`. Otherwise, set `answer[i]` to `false`.

Return the array `answer`.

A **palindrome** is a string that reads the same forward and backward.

**Example 1:**

![](https://assets.leetcode.com/uploads/2024/09/01/tree1drawio.png)

**Input:** parent = [-1,0,0,1,1,2], s = "aababa"

**Output:** [true,true,false,true,true,true]

**Explanation:**

*   Calling `dfs(0)` results in the string `dfsStr = "abaaba"`, which is a palindrome.
*   Calling `dfs(1)` results in the string `dfsStr = "aba"`, which is a palindrome.
*   Calling `dfs(2)` results in the string `dfsStr = "ab"`, which is **not** a palindrome.
*   Calling `dfs(3)` results in the string `dfsStr = "a"`, which is a palindrome.
*   Calling `dfs(4)` results in the string `dfsStr = "b"`, which is a palindrome.
*   Calling `dfs(5)` results in the string `dfsStr = "a"`, which is a palindrome.

**Example 2:**

![](https://assets.leetcode.com/uploads/2024/09/01/tree2drawio-1.png)

**Input:** parent = [-1,0,0,0,0], s = "aabcb"

**Output:** [true,true,true,true,true]

**Explanation:**

Every call on `dfs(x)` results in a palindrome string.

**Constraints:**

*   `n == parent.length == s.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `0 <= parent[i] <= n - 1` for all `i >= 1`.
*   `parent[0] == -1`
*   `parent` represents a valid tree.
*   `s` consists only of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private final List<List<Integer>> e = new ArrayList<>();
    private final StringBuilder stringBuilder = new StringBuilder();
    private String s;
    private int now;
    private int n;
    private int[] l;
    private int[] r;
    private int[] p;
    private char[] c;

    private void dfs(int x) {
        l[x] = now + 1;
        for (int v : e.get(x)) {
            dfs(v);
        }
        stringBuilder.append(s.charAt(x));
        r[x] = ++now;
    }

    private void matcher() {
        c[0] = '~';
        c[1] = '#';
        for (int i = 1; i <= n; ++i) {
            c[2 * i + 1] = '#';
            c[2 * i] = stringBuilder.charAt(i - 1);
        }
        int j = 1;
        int mid = 0;
        int localR = 0;
        while (j <= 2 * n + 1) {
            if (j <= localR) {
                p[j] = Math.min(p[(mid << 1) - j], localR - j + 1);
            }
            while (c[j - p[j]] == c[j + p[j]]) {
                ++p[j];
            }
            if (p[j] + j > localR) {
                localR = p[j] + j - 1;
                mid = j;
            }
            ++j;
        }
    }

    public boolean[] findAnswer(int[] parent, String s) {
        n = parent.length;
        this.s = s;
        for (int i = 0; i < n; ++i) {
            e.add(new ArrayList<>());
        }
        for (int i = 1; i < n; ++i) {
            e.get(parent[i]).add(i);
        }
        l = new int[n];
        r = new int[n];
        dfs(0);
        c = new char[2 * n + 10];
        p = new int[2 * n + 10];
        matcher();
        boolean[] ans = new boolean[n];
        for (int i = 0; i < n; ++i) {
            int mid = (2 * r[i] - 2 * l[i] + 1) / 2 + 2 * l[i];
            ans[i] = p[mid] - 1 >= r[i] - l[i] + 1;
        }
        return ans;
    }
}
```