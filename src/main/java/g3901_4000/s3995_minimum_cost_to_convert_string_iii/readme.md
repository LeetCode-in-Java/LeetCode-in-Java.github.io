[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3995\. Minimum Cost to Convert String III

Hard

You are given two strings, `source` and `target`.

You are also given a 2D string array `rules`, where <code>rules[i] = [pattern<sub>i</sub>, replacement<sub>i</sub>]</code>, and an integer array `costs`, where `costs[i]` is the base cost of applying `rules[i]`. Both arrays have the same length. Additionally, <code>pattern<sub>i</sub></code> and <code>replacement<sub>i</sub></code> have the same length.

You may apply **any** rule **any** number of times. Each rule application works as follows:

*   Choose an index `l` such that the range of positions from `l` to <code>l + pattern<sub>i</sub>.length - 1</code> exists in the current string and **none** of these positions has been used in a previous rule application.
*   For each index `j`, the character <code>pattern<sub>i</sub>[j]</code> must either be **equal** to the current character at position `l + j`, or be `'*'`.
*   Replace the characters in this range with <code>replacement<sub>i</sub></code>. The replacement is used **exactly** as given and does not contain wildcards.
*   The cost of this rule application is `costs[i]` **plus** the number of `'*'` characters in <code>pattern<sub>i</sub></code>.
*   Once a character position has been used in a rule application, it **cannot** be used in any **later** rule application.

Since every <code>pattern<sub>i</sub></code> and <code>replacement<sub>i</sub></code> have the same length, character positions are preserved after every rule application.

Return the **minimum** total cost required to transform `source` into `target`. If it is impossible, return -1.

OriginalJava**Example 1:**

**Input:** source = "hello", target = "world", rules = \[\["he","wo"],["llo","rld"]], costs = [3,4]

**Output:** 7

**Explanation:**

*   Apply `rules[0]` to replace `"he"` with `"wo"` at cost 3, so the string becomes `"wollo"`.
*   Apply `rules[1]` to replace `"llo"` with `"rld"` at cost 4, so the string becomes `"world"`.
*   The total cost is `3 + 4 = 7`.

**Example 2:**

**Input:** source = "cat", target = "dog", rules = \[\["c\*t","dog"]], costs = [2]

**Output:** 3

**Explanation:**

*   Apply `rules[0]` to replace `"cat"` with `"dog"`. The wildcard `'*'` matches `'a'`, adding 1 to the base cost 2.
*   The total cost is `2 + 1 = 3`.

**Example 3:**

**Input:** source = "test", target = "next", rules = \[\["\*e\*t","next"]], costs = [4]

**Output:** 6

**Explanation:**

*   Apply `rules[0]` to replace `"test"` with `"next"`. The first wildcard matches `'t'` and the second wildcard matches `'s'`, adding 2 to the base cost 4.
*   The total cost is `4 + 2 = 6`.

**Example 4:**

**Input:** source = "ab", target = "bc", rules = \[\["a\*","bd"]], costs = [9]

**Output:** \-1

**Explanation:**

No sequence of rule applications can transform `source` into `target`, so the answer is -1.

**Constraints:**

*   `1 <= source.length == target.length <= 5000`
*   `source` and `target` consist of lowercase English letters.
*   `1 <= rules.length == costs.length <= 200`
*   <code>rules[i] = [pattern<sub>i</sub>, replacement<sub>i</sub>]</code>
*   <code>1 <= pattern<sub>i</sub>.length == replacement<sub>i</sub>.length <= 20</code>
*   <code>pattern<sub>i</sub></code> contains at least one lowercase English letter and at most 5 `'*'` characters.
*   <code>replacement<sub>i</sub></code> contains only lowercase English letters.
*   `1 <= costs[i] <= 1000`

## Solution

```java
import java.util.Arrays;
import java.util.List;

public class Solution {
    public int minCost(String source, String target, List<List<String>> rules, int[] costs) {
        int n = source.length();
        if (n != target.length()) {
            return -1;
        }
        if (source.equals(target)) {
            return 0;
        }
        int m = rules.size();
        int[] len = new int[m];
        String[] pat = new String[m];
        String[] rep = new String[m];
        int[] rulesCost = new int[m];
        for (int i = 0; i < m; i++) {
            String p = rules.get(i).get(0);
            String r = rules.get(i).get(1);
            pat[i] = p;
            rep[i] = r;
            len[i] = p.length();
            int wild = 0;
            for (char c : p.toCharArray()) {
                if (c == '*') {
                    wild++;
                }
            }
            rulesCost[i] = costs[i] + wild;
        }
        int in = Integer.MAX_VALUE / 2;
        int[] dp = new int[n + 1];
        Arrays.fill(dp, in);
        dp[0] = 0;
        for (int i = 0; i < n; i++) {
            if (dp[i] == in) {
                continue;
            }
            if (source.charAt(i) == target.charAt(i) && dp[i] < dp[i + 1]) {
                dp[i + 1] = dp[i];
            }
            for (int k = 0; k < m; k++) {
                int l = len[k];
                if (i + l > n) {
                    continue;
                }
                boolean match = true;
                for (int j = 0; j < l; j++) {
                    char pc = pat[k].charAt(j);
                    char sc = source.charAt(i + j);
                    if (pc != '*' && pc != sc) {
                        match = false;
                        break;
                    }
                }
                if (!match) {
                    continue;
                }
                if (!target.substring(i, i + l).equals(rep[k])) {
                    continue;
                }
                int nd = dp[i] + rulesCost[k];
                if (nd < dp[i + l]) {
                    dp[i + l] = nd;
                }
            }
        }
        return dp[n] >= in ? -1 : dp[n];
    }
}
```