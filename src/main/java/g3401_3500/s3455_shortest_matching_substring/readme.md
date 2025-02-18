[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3455\. Shortest Matching Substring

Hard

You are given a string `s` and a pattern string `p`, where `p` contains **exactly two** `'*'` characters.

The `'*'` in `p` matches any sequence of zero or more characters.

Return the length of the **shortest** **substring** in `s` that matches `p`. If there is no such substring, return -1.

**Note:** The empty substring is considered valid.

**Example 1:**

**Input:** s = "abaacbaecebce", p = "ba\*c\*ce"

**Output:** 8

**Explanation:**

The shortest matching substring of `p` in `s` is <code>"<ins>**ba**</ins>e<ins>**c**</ins>eb<ins>**ce**</ins>"</code>.

**Example 2:**

**Input:** s = "baccbaadbc", p = "cc\*baa\*adb"

**Output:** \-1

**Explanation:**

There is no matching substring in `s`.

**Example 3:**

**Input:** s = "a", p = "\*\*"

**Output:** 0

**Explanation:**

The empty substring is the shortest matching substring.

**Example 4:**

**Input:** s = "madlogic", p = "\*adlogi\*"

**Output:** 6

**Explanation:**

The shortest matching substring of `p` in `s` is <code>"**<ins>adlogi</ins>**"</code>.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   <code>2 <= p.length <= 10<sup>5</sup></code>
*   `s` contains only lowercase English letters.
*   `p` contains only lowercase English letters and exactly two `'*'`.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    private List<Integer> getMatch(String s, String p) {
        int n = s.length();
        int m = p.length();
        int[] next = new int[m];
        Arrays.fill(next, -1);
        int i = 1;
        int j = -1;
        while (i < m) {
            while (j != -1 && p.charAt(i) != p.charAt(j + 1)) {
                j = next[j];
            }
            if (p.charAt(i) == p.charAt(j + 1)) {
                ++j;
            }
            next[i] = j;
            ++i;
        }
        List<Integer> match = new ArrayList<>();
        i = 0;
        j = -1;
        while (i < n) {
            while (j != -1 && s.charAt(i) != p.charAt(j + 1)) {
                j = next[j];
            }
            if (s.charAt(i) == p.charAt(j + 1)) {
                ++j;
            }
            if (j == m - 1) {
                match.add(i - m + 1);
                j = next[j];
            }
            ++i;
        }
        return match;
    }

    public int shortestMatchingSubstring(String s, String p) {
        int n = s.length();
        int m = p.length();
        int[] d = {-1, -1, -1, m};
        for (int i = 0; i < m; ++i) {
            if (p.charAt(i) == '*') {
                d[d[1] == -1 ? 1 : 2] = i;
            }
        }
        List<String> subs = new ArrayList<>();
        for (int i = 0; i < 3; ++i) {
            if (d[i] + 1 < d[i + 1]) {
                subs.add(p.substring(d[i] + 1, d[i + 1]));
            }
        }
        int size = subs.size();
        if (size == 0) {
            return 0;
        }
        List<List<Integer>> matches = new ArrayList<>();
        for (String sub : subs) {
            matches.add(getMatch(s, sub));
        }
        int ans = Integer.MAX_VALUE;
        int[] ids = new int[size];
        Arrays.fill(ids, 0);
        while (ids[size - 1] < matches.get(size - 1).size()) {
            for (int i = size - 2; i >= 0; --i) {
                while (ids[i] + 1 < matches.get(i).size()
                        && matches.get(i).get(ids[i] + 1) + subs.get(i).length()
                                <= matches.get(i + 1).get(ids[i + 1])) {
                    ++ids[i];
                }
            }
            boolean valid = true;
            for (int i = size - 2; i >= 0; --i) {
                if (ids[i] >= matches.get(i).size()
                        || matches.get(i).get(ids[i]) + subs.get(i).length()
                                > matches.get(i + 1).get(ids[i + 1])) {
                    valid = false;
                    break;
                }
            }
            if (valid) {
                ans =
                        Math.min(
                                ans,
                                matches.get(size - 1).get(ids[size - 1])
                                        + subs.get(size - 1).length()
                                        - matches.get(0).get(ids[0]));
            }
            ids[size - 1]++;
        }
        return ans > n ? -1 : ans;
    }
}
```