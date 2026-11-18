[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3999\. Minimum Number of String Groups Through Transformations

Hard

You are given an array of strings `words`.

Define a **transformation** on a string `s` as follows:

*   Let `E` be the subsequence of characters at even indices of `s`.
*   Let `O` be the **subsequence** of characters at odd indices of `s`.
*   **Independently** cyclically shift `E` and `O` by **any** number of positions to the right, possibly zero.
*   Reconstruct the string by placing the shifted `E` characters back into even indices and the shifted `O` characters back into odd indices.

Two strings are **equivalent** if one can be transformed into the other by a **single** transformation.

Partition `words` into the **minimum** number of groups such that:

*   Every string belongs to **exactly** one group.
*   Every pair of strings in the same group are **equivalent**.

Return an integer denoting the **minimum** number of groups.

**Example 1:**

**Input:** words = ["ntgwz","zwntg"]

**Output:** 1

**Explanation:**

*   For `"ntgwz"`, the even-index subsequence is `"ngz"` and the odd-index subsequence is `"tw"`.
*   Shift `"ngz"` right by `1` position to obtain `"zng"`, and shift `"tw"` right by `1` position to obtain `"wt"`.
*   After reconstructing the string, we obtain `"zwntg"`.
*   Therefore, both strings are equivalent and belong to the same group.

**Example 2:**

**Input:** words = ["abc","cab","bac","acb","bca","cba"]

**Output:** 3

**Explanation:**

The strings can be partitioned into the following groups:

*   `["abc","cba"]`
*   `["cab","bac"]`
*   `["acb","bca"]`

**Example 3:**

**Input:** words = ["leet","abb","bab","deed","edde","code","bba"]

**Output:** 5

**Explanation:**

The strings can be partitioned into the following groups:

*   `["abb","bba"]`
*   `["deed","edde"]`
*   `["leet"]`
*   `["bab"]`
*   `["code"]`

All pairs of strings in each group are equivalent.

**Constraints:**

*   <code>1 <= words.length <= 10<sup>5</sup></code>
*   <code>1 <= words[i].length <= 5 * 10<sup>5</sup></code>
*   The sum of `words[i].length` does not exceed <code>5 * 10<sup>5</sup></code>.
*   `words[i]` consist of lowercase English letters.

## Solution

```java
import java.util.HashSet;

public class Solution {
    public int minimumGroups(String[] words) {
        HashSet<Long> set = new HashSet<>();
        for (String w : words) {
            int n = w.length();
            int evenLen = (n + 1) >> 1;
            int oddLen = n >> 1;
            char[] even = new char[evenLen];
            char[] odd = new char[oddLen];
            int ei = 0;
            int oi = 0;
            for (int i = 0; i < n; i++) {
                if ((i & 1) == 0) {
                    even[ei++] = w.charAt(i);
                } else {
                    odd[oi++] = w.charAt(i);
                }
            }
            int eStart = booth(even);
            int oStart = booth(odd);
            long h1 = hash(even, eStart);
            long h2 = hash(odd, oStart);
            long key = h1 * 1000003L + h2;
            set.add(key);
        }
        return set.size();
    }

    private int booth(char[] s) {
        int n = s.length;
        if (n <= 1) {
            return 0;
        }
        int i = 0;
        int j = 1;
        int k = 0;
        while (i < n && j < n && k < n) {
            char a = s[(i + k) % n];
            char b = s[(j + k) % n];
            if (a == b) {
                k++;
            } else if (a > b) {
                i += k + 1;
                if (i <= j) {
                    i = j + 1;
                }
                k = 0;
            } else {
                j += k + 1;
                if (j <= i) {
                    j = i + 1;
                }
                k = 0;
            }
        }
        return Math.min(i, j);
    }

    private long hash(char[] s, int start) {
        long h = 0;
        for (int i = 0; i < s.length; i++) {
            h = h * 131 + s[(start + i) % s.length];
        }
        return h;
    }
}
```