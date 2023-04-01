[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1044\. Longest Duplicate Substring

Hard

Given a string `s`, consider all _duplicated substrings_: (contiguous) substrings of s that occur 2 or more times. The occurrences may overlap.

Return **any** duplicated substring that has the longest possible length. If `s` does not have a duplicated substring, the answer is `""`.

**Example 1:**

**Input:** s = "banana"

**Output:** "ana"

**Example 2:**

**Input:** s = "abcd"

**Output:** ""

**Constraints:**

*   <code>2 <= s.length <= 3 * 10<sup>4</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

@SuppressWarnings("unchecked")
public class Solution {
    private static final int MOD = (int) 1e9 + 7;
    private long[] hsh;
    private long[] pw;
    private final List[] cnt = new List[26];

    public String longestDupSubstring(String s) {
        int n = s.length();
        int base = 131;
        for (int i = 0; i < 26; i++) {
            cnt[i] = new ArrayList<>();
        }
        hsh = new long[n + 1];
        pw = new long[n + 1];
        pw[0] = 1;
        for (int j = 1; j <= n; j++) {
            hsh[j] = (hsh[j - 1] * base + s.charAt(j - 1)) % MOD;
            pw[j] = pw[j - 1] * base % MOD;
            cnt[s.charAt(j - 1) - 'a'].add(j - 1);
        }
        String ans = "";
        for (int i = 0; i < 26; i++) {
            if (cnt[i].isEmpty()) {
                continue;
            }
            List<Integer> idx = cnt[i];
            Set<Long> set;
            int lo = 1;
            int hi = n - idx.get(0);
            while (lo <= hi) {
                int len = (lo + hi) / 2;
                set = new HashSet<>();
                boolean found = false;
                for (int nxt : idx) {
                    if (nxt + len <= n) {
                        long substrHash = getSubstrHash(nxt, nxt + len);
                        if (set.contains(substrHash)) {
                            found = true;
                            if (len + 1 > ans.length()) {
                                ans = s.substring(nxt, nxt + len);
                            }
                            break;
                        }
                        set.add(substrHash);
                    }
                }
                if (found) {
                    lo = len + 1;
                } else {
                    hi = len - 1;
                }
            }
        }
        return ans;
    }

    private long getSubstrHash(int l, int r) {
        return (hsh[r] - hsh[l] * pw[r - l] % MOD + MOD) % MOD;
    }
}
```