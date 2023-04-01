[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1202\. Smallest String With Swaps

Medium

You are given a string `s`, and an array of pairs of indices in the string `pairs` where `pairs[i] = [a, b]` indicates 2 indices(0-indexed) of the string.

You can swap the characters at any pair of indices in the given `pairs` **any number of times**.

Return the lexicographically smallest string that `s` can be changed to after using the swaps.

**Example 1:**

**Input:** s = "dcab", pairs = \[\[0,3],[1,2]]

**Output:** "bacd" **Explaination:** 

Swap s[0] and s[3], s = "bcad" 

Swap s[1] and s[2], s = "bacd"

**Example 2:**

**Input:** s = "dcab", pairs = \[\[0,3],[1,2],[0,2]]

**Output:** "abcd" **Explaination:**  

Swap s[0] and s[3], s = "bcad" 

Swap s[0] and s[2], s = "acbd" 

Swap s[1] and s[2], s = "abcd"

**Example 3:**

**Input:** s = "cba", pairs = \[\[0,1],[1,2]]

**Output:** "abc" **Explaination:**  

Swap s[0] and s[1], s = "bca" 

Swap s[1] and s[2], s = "bac" 

Swap s[0] and s[1], s = "abc"

**Constraints:**

*   `1 <= s.length <= 10^5`
*   `0 <= pairs.length <= 10^5`
*   `0 <= pairs[i][0], pairs[i][1] < s.length`
*   `s` only contains lower case English letters.

## Solution

```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public String smallestStringWithSwaps(String s, List<List<Integer>> pairs) {
        UF uf = new UF(s.length());
        for (List<Integer> p : pairs) {
            uf.union(p.get(0), p.get(1));
        }

        Map<Integer, int[]> freqMapPerRoot = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            freqMapPerRoot.computeIfAbsent(uf.find(i), x -> new int[26])[s.charAt(i) - 'a']++;
        }

        char[] ans = new char[s.length()];
        for (int i = 0; i < ans.length; i++) {
            int[] css = freqMapPerRoot.get(uf.find(i));
            for (int j = 0; j < css.length; j++) {
                if (css[j] > 0) {
                    ans[i] = (char) (j + 'a');
                    css[j]--;
                    break;
                }
            }
        }
        return new String(ans);
    }

    static class UF {
        int[] root;
        int[] rank;

        UF(int n) {
            root = new int[n];
            rank = new int[n];
            for (int i = 0; i < n; i++) {
                root[i] = i;
                rank[i] = 1;
            }
        }

        int find(int u) {
            if (u == root[u]) {
                return u;
            }

            root[u] = find(root[u]);
            return root[u];
        }

        void union(int u, int v) {
            int ru = find(root[u]);
            int rv = find(root[v]);
            if (ru != rv) {
                if (rank[ru] < rank[rv]) {
                    root[ru] = root[rv];
                } else if (rank[ru] > rank[rv]) {
                    root[rv] = root[ru];
                } else {
                    root[rv] = root[ru];
                    rank[ru]++;
                }
            }
        }
    }
}
```