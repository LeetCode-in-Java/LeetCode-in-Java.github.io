[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3458\. Select K Disjoint Special Substrings

Medium

Given a string `s` of length `n` and an integer `k`, determine whether it is possible to select `k` disjoint **special substrings**.

A **special substring** is a **substring** where:

*   Any character present inside the substring should not appear outside it in the string.
*   The substring is not the entire string `s`.

**Note** that all `k` substrings must be disjoint, meaning they cannot overlap.

Return `true` if it is possible to select `k` such disjoint special substrings; otherwise, return `false`.

**Example 1:**

**Input:** s = "abcdbaefab", k = 2

**Output:** true

**Explanation:**

*   We can select two disjoint special substrings: `"cd"` and `"ef"`.
*   `"cd"` contains the characters `'c'` and `'d'`, which do not appear elsewhere in `s`.
*   `"ef"` contains the characters `'e'` and `'f'`, which do not appear elsewhere in `s`.

**Example 2:**

**Input:** s = "cdefdc", k = 3

**Output:** false

**Explanation:**

There can be at most 2 disjoint special substrings: `"e"` and `"f"`. Since `k = 3`, the output is `false`.

**Example 3:**

**Input:** s = "abeabe", k = 0

**Output:** true

**Constraints:**

*   <code>2 <= n == s.length <= 5 * 10<sup>4</sup></code>
*   `0 <= k <= 26`
*   `s` consists only of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    public boolean maxSubstringLength(String s, int k) {
        int n = s.length();
        if (k == 0) {
            return true;
        }
        int[] first = new int[26];
        int[] last = new int[26];
        Arrays.fill(first, n);
        Arrays.fill(last, -1);
        for (int i = 0; i < n; i++) {
            int c = s.charAt(i) - 'a';
            first[c] = Math.min(first[c], i);
            last[c] = i;
        }
        List<int[]> intervals = new ArrayList<>();
        for (int c = 0; c < 26; c++) {
            if (last[c] == -1) {
                continue;
            }
            int start = first[c];
            int end = last[c];
            int j = start;
            boolean valid = true;
            while (j <= end) {
                int cur = s.charAt(j) - 'a';
                if (first[cur] < start) {
                    valid = false;
                    break;
                }
                end = Math.max(end, last[cur]);
                j++;
            }
            if (valid && !(start == 0 && end == n - 1)) {
                intervals.add(new int[] {start, end});
            }
        }
        intervals.sort((a, b) -> Integer.compare(a[1], b[1]));
        int count = 0;
        int prevEnd = -1;
        for (int[] interval : intervals) {
            if (interval[0] > prevEnd) {
                count++;
                prevEnd = interval[1];
            }
        }
        return count >= k;
    }
}
```