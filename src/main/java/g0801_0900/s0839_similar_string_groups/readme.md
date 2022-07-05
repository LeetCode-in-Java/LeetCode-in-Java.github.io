[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 839\. Similar String Groups

Hard

Two strings `X` and `Y` are similar if we can swap two letters (in different positions) of `X`, so that it equals `Y`. Also two strings `X` and `Y` are similar if they are equal.

For example, `"tars"` and `"rats"` are similar (swapping at positions `0` and `2`), and `"rats"` and `"arts"` are similar, but `"star"` is not similar to `"tars"`, `"rats"`, or `"arts"`.

Together, these form two connected groups by similarity: `{"tars", "rats", "arts"}` and `{"star"}`. Notice that `"tars"` and `"arts"` are in the same group even though they are not similar. Formally, each group is such that a word is in the group if and only if it is similar to at least one other word in the group.

We are given a list `strs` of strings where every string in `strs` is an anagram of every other string in `strs`. How many groups are there?

**Example 1:**

**Input:** strs = ["tars","rats","arts","star"]

**Output:** 2

**Example 2:**

**Input:** strs = ["omv","ovm"]

**Output:** 1

**Constraints:**

*   `1 <= strs.length <= 300`
*   `1 <= strs[i].length <= 300`
*   `strs[i]` consists of lowercase letters only.
*   All words in `strs` have the same length and are anagrams of each other.

## Solution

```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int numSimilarGroups(String[] strs) {
        boolean[] visited = new boolean[strs.length];
        int res = 0;
        for (int i = 0; i < strs.length; i++) {
            if (!visited[i]) {
                res++;
                bfs(i, visited, strs);
            }
        }
        return res;
    }

    private void bfs(int i, boolean[] visited, String[] strs) {
        Queue<String> qu = new LinkedList<>();
        qu.add(strs[i]);
        visited[i] = true;
        while (!qu.isEmpty()) {
            String s = qu.poll();
            for (int j = 0; j < strs.length; j++) {
                if (visited[j]) {
                    continue;
                }
                if (isSimilar(s, strs[j])) {
                    visited[j] = true;
                    qu.add(strs[j]);
                }
            }
        }
    }

    private boolean isSimilar(String s1, String s2) {
        Character c1 = null;
        Character c2 = null;
        int mismatchCount = 0;
        for (int i = 0; i < s1.length(); i++) {
            if (s1.charAt(i) == s2.charAt(i)) {
                continue;
            }
            mismatchCount++;
            if (c1 == null) {
                c1 = s1.charAt(i);
                c2 = s2.charAt(i);
            } else if (s2.charAt(i) != c1 || s1.charAt(i) != c2) {
                return false;
            } else if (mismatchCount > 2) {
                return false;
            }
        }
        return true;
    }
}
```