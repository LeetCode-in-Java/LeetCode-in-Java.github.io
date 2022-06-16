## 2301\. Match Substring After Replacement

Hard

You are given two strings `s` and `sub`. You are also given a 2D character array `mappings` where <code>mappings[i] = [old<sub>i</sub>, new<sub>i</sub>]</code> indicates that you may **replace** any number of <code>old<sub>i</sub></code> characters of `sub` with <code>new<sub>i</sub></code>. Each character in `sub` **cannot** be replaced more than once.

Return `true` _if it is possible to make_ `sub` _a substring of_ `s` _by replacing zero or more characters according to_ `mappings`. Otherwise, return `false`.

A **substring** is a contiguous non-empty sequence of characters within a string.

**Example 1:**

**Input:** s = "fool3e7bar", sub = "leet", mappings = \[\["e","3"],["t","7"],["t","8"]]

**Output:** true

**Explanation:** Replace the first 'e' in sub with '3' and 't' in sub with '7'.

Now sub = "l3e7" is a substring of s, so we return true.

**Example 2:**

**Input:** s = "fooleetbar", sub = "f00l", mappings = \[\["o","0"]]

**Output:** false

**Explanation:** The string "f00l" is not a substring of s and no replacements can be made.

Note that we cannot replace '0' with 'o'. 

**Example 3:**

**Input:** s = "Fool33tbaR", sub = "leetd", mappings = \[\["e","3"],["t","7"],["t","8"],["d","b"],["p","b"]]

**Output:** true

**Explanation:** Replace the first and second 'e' in sub with '3' and 'd' in sub with 'b'.

Now sub = "l33tb" is a substring of s, so we return true. 

**Constraints:**

*   `1 <= sub.length <= s.length <= 5000`
*   `0 <= mappings.length <= 1000`
*   `mappings[i].length == 2`
*   <code>old<sub>i</sub> != new<sub>i</sub></code>
*   `s` and `sub` consist of uppercase and lowercase English letters and digits.
*   <code>old<sub>i</sub></code> and <code>new<sub>i</sub></code> are either uppercase or lowercase English letters or digits.

## Solution

```java
import java.util.HashSet;
import java.util.Set;

@SuppressWarnings("unchecked")
public class Solution {
    private char[] c1;
    private char[] c2;
    private Set<Character>[] al;

    public boolean matchReplacement(String s, String sub, char[][] mappings) {
        c1 = s.toCharArray();
        c2 = sub.toCharArray();
        al = new Set[75];
        for (int i = 0; i < 75; i++) {
            Set<Character> temp = new HashSet<>();
            al[i] = temp;
        }
        for (char[] mapping : mappings) {
            al[mapping[0] - '0'].add(mapping[1]);
        }
        return ans(c1.length, c2.length) == 1;
    }

    private int ans(int m, int n) {
        if (m == 0) {
            return 0;
        }
        if (ans(m - 1, n) == 1) {
            return 1;
        }
        if (m >= n && (c1[m - 1] == c2[n - 1] || al[c2[n - 1] - '0'].contains(c1[m - 1]))) {
            while (n >= 1 && (c1[m - 1] == c2[n - 1] || al[c2[n - 1] - '0'].contains(c1[m - 1]))) {
                n--;
                m--;
            }
            if (n == 0) {
                return 1;
            }
        }
        return 0;
    }
}
```