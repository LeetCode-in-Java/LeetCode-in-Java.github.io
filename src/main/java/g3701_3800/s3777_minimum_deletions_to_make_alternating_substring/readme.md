[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3777\. Minimum Deletions to Make Alternating Substring

Hard

You are given a string `s` of length `n` consisting only of the characters `'A'` and `'B'`.

You are also given a 2D integer array `queries` of length `q`, where each `queries[i]` is one of the following:

*   `[1, j]`: **Flip** the character at index `j` of `s` i.e. `'A'` changes to `'B'` (and vice versa). This operation mutates `s` and affects subsequent queries.
*   `[2, l, r]`: **Compute** the **minimum** number of character deletions required to make the **substring** `s[l..r]` **alternating**. This operation does not modify `s`; the length of `s` remains `n`.

A **substring** is **alternating** if no two **adjacent** characters are **equal**. A substring of length 1 is always alternating.

Return an integer array `answer`, where `answer[i]` is the result of the <code>i<sup>th</sup></code> query of type `[2, l, r]`.

**Example 1:**

**Input:** s = "ABA", queries = \[\[2,1,2],[1,1],[2,0,2]]

**Output:** [0,2]

**Explanation:**

| i | queries[i] | j | l | r | s before query | s[l..r] | Result | Answer |
|---|------------|---|---|---|----------------|---------|--------|--------|
| 0 | [2, 1, 2]  | - | 1 | 2 | `"ABA"`        | `"BA"`  | Already alternating | 0 |
| 1 | [1, 1]     | 1 | - | - | `"ABA"`        | -       | Flip `s[1]` from `'B'` to `'A'` | - |
| 2 | [2, 0, 2]  | - | 0 | 2 | `"AAA"`        | `"AAA"` | Delete any two `'A'`s to get `"A"` | 2 |

Thus, the answer is `[0, 2]`.

**Example 2:**

**Input:** s = "ABB", queries = \[\[2,0,2],[1,2],[2,0,2]]

**Output:** [1,0]

**Explanation:**

| i | queries[i] | j | l | r | s before query | s[l..r] | Result | Answer |
|---|------------|---|---|---|----------------|---------|--------|--------|
| 0 | [2, 0, 2]  | - | 0 | 2 | `"ABB"`        | `"ABB"` | Delete one `'B'` to get `"AB"` | 1 |
| 1 | [1, 2]     | 2 | - | - | `"ABB"`        | -       | Flip `s[2]` from `'B'` to `'A'` | - |
| 2 | [2, 0, 2]  | - | 0 | 2 | `"ABA"`        | `"ABA"` | Already alternating | 0 |

Thus, the answer is `[1, 0]`.

**Example 3:**

**Input:** s = "BABA", queries = \[\[2,0,3],[1,1],[2,1,3]]

**Output:** [0,1]

**Explanation:**

| i | queries[i] | j | l | r | s before query | s[l..r] | Result | Answer |
|---|------------|---|---|---|----------------|---------|--------|--------|
| 0 | [2, 0, 3]  | - | 0 | 3 | `"BABA"`       | `"BABA"`| Already alternating | 0 |
| 1 | [1, 1]     | 1 | - | - | `"BABA"`       | -       | Flip `s[1]` from `'A'` to `'B'` | - |
| 2 | [2, 1, 3]  | - | 1 | 3 | `"BBBA"`       | `"BBA"` | Delete one `'B'` to get `"BA"` | 1 |

Thus, the answer is `[0, 1]`.

**Constraints:**

*   <code>1 <= n == s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'A'` or `'B'`.
*   <code>1 <= q == queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 2` or `3`
    *   `queries[i] == [1, j]` or,
    *   `queries[i] == [2, l, r]`
    *   `0 <= j <= n - 1`
    *   `0 <= l <= r <= n - 1`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int[] f;
    private int n;

    private void add(int i, int v) {
        for (i++; i <= n; i += i & -i) {
            f[i] += v;
        }
    }

    private int sum(int i) {
        int s = 0;
        for (i++; i > 0; i -= i & -i) {
            s += f[i];
        }
        return s;
    }

    public int[] minDeletions(String s, int[][] q) {
        char[] a = s.toCharArray();
        n = a.length;
        f = new int[n + 1];
        for (int i = 0; i + 1 < n; i++) {
            if (a[i] == a[i + 1]) {
                add(i, 1);
            }
        }
        int[] ans = new int[q.length];
        int k = 0;
        for (int[] e : q) {
            if (e[0] == 1) {
                int j = e[1];
                if (j > 0 && a[j] == a[j - 1]) {
                    add(j - 1, -1);
                }
                if (j + 1 < n && a[j] == a[j + 1]) {
                    add(j, -1);
                }
                a[j] = a[j] == 'A' ? 'B' : 'A';
                if (j > 0 && a[j] == a[j - 1]) {
                    add(j - 1, 1);
                }
                if (j + 1 < n && a[j] == a[j + 1]) {
                    add(j, 1);
                }
            } else {
                int l = e[1];
                int r = e[2];
                ans[k++] = l < r ? sum(r - 1) - sum(l - 1) : 0;
            }
        }
        return Arrays.copyOf(ans, k);
    }
}
```