[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3598\. Longest Common Prefix Between Adjacent Strings After Removals

Medium

You are given an array of strings `words`. For each index `i` in the range `[0, words.length - 1]`, perform the following steps:

*   Remove the element at index `i` from the `words` array.
*   Compute the **length** of the **longest common prefix** among all **adjacent** pairs in the modified array.

Return an array `answer`, where `answer[i]` is the length of the longest common prefix between the adjacent pairs after removing the element at index `i`. If **no** adjacent pairs remain or if **none** share a common prefix, then `answer[i]` should be 0.

**Example 1:**

**Input:** words = ["jump","run","run","jump","run"]

**Output:** [3,0,0,3,3]

**Explanation:**

*   Removing index 0:
    *   `words` becomes `["run", "run", "jump", "run"]`
    *   Longest adjacent pair is `["run", "run"]` having a common prefix `"run"` (length 3)
*   Removing index 1:
    *   `words` becomes `["jump", "run", "jump", "run"]`
    *   No adjacent pairs share a common prefix (length 0)
*   Removing index 2:
    *   `words` becomes `["jump", "run", "jump", "run"]`
    *   No adjacent pairs share a common prefix (length 0)
*   Removing index 3:
    *   `words` becomes `["jump", "run", "run", "run"]`
    *   Longest adjacent pair is `["run", "run"]` having a common prefix `"run"` (length 3)
*   Removing index 4:
    *   words becomes `["jump", "run", "run", "jump"]`
    *   Longest adjacent pair is `["run", "run"]` having a common prefix `"run"` (length 3)

**Example 2:**

**Input:** words = ["dog","racer","car"]

**Output:** [0,0,0]

**Explanation:**

*   Removing any index results in an answer of 0.

**Constraints:**

*   <code>1 <= words.length <= 10<sup>5</sup></code>
*   <code>1 <= words[i].length <= 10<sup>4</sup></code>
*   `words[i]` consists of lowercase English letters.
*   The sum of `words[i].length` is smaller than or equal <code>10<sup>5</sup></code>.

## Solution

```java
public class Solution {
    private int solve(String a, String b) {
        int len = Math.min(a.length(), b.length());
        int cnt = 0;
        while (cnt < len && a.charAt(cnt) == b.charAt(cnt)) {
            cnt++;
        }
        return cnt;
    }

    public int[] longestCommonPrefix(String[] words) {
        int n = words.length;
        int[] ans = new int[n];
        if (n <= 1) {
            return ans;
        }
        int[] lcp = new int[n - 1];
        for (int i = 0; i + 1 < n; i++) {
            lcp[i] = solve(words[i], words[i + 1]);
        }
        int[] prefmax = new int[n - 1];
        int[] sufmax = new int[n - 1];
        prefmax[0] = lcp[0];
        for (int i = 1; i < n - 1; i++) {
            prefmax[i] = Math.max(prefmax[i - 1], lcp[i]);
        }
        sufmax[n - 2] = lcp[n - 2];
        for (int i = n - 3; i >= 0; i--) {
            sufmax[i] = Math.max(sufmax[i + 1], lcp[i]);
        }
        for (int i = 0; i < n; i++) {
            int best = 0;
            if (i >= 2) {
                best = Math.max(best, prefmax[i - 2]);
            }
            if (i + 1 <= n - 2) {
                best = Math.max(best, sufmax[i + 1]);
            }
            if (i > 0 && i < n - 1) {
                best = Math.max(best, solve(words[i - 1], words[i + 1]));
            }
            ans[i] = best;
        }
        return ans;
    }
}
```