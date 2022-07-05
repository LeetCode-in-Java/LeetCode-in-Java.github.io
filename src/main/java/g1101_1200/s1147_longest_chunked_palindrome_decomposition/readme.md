[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1147\. Longest Chunked Palindrome Decomposition

Hard

You are given a string `text`. You should split it to k substrings <code>(subtext<sub>1</sub>, subtext<sub>2</sub>, ..., subtext<sub>k</sub>)</code> such that:

*   <code>subtext<sub>i</sub></code> is a **non-empty** string.
*   The concatenation of all the substrings is equal to `text` (i.e., <code>subtext<sub>1</sub> + subtext<sub>2</sub> + ... + subtext<sub>k</sub> == text</code>).
*   <code>subtext<sub>i</sub> == subtext<sub>k - i + 1</sub></code> for all valid values of `i` (i.e., `1 <= i <= k`).

Return the largest possible value of `k`.

**Example 1:**

**Input:** text = "ghiabcdefhelloadamhelloabcdefghi"

**Output:** 7

**Explanation:** We can split the string on "(ghi)(abcdef)(hello)(adam)(hello)(abcdef)(ghi)".

**Example 2:**

**Input:** text = "merchant"

**Output:** 1

**Explanation:** We can split the string on "(merchant)".

**Example 3:**

**Input:** text = "antaprezatepzapreanta"

**Output:** 11

**Explanation:** We can split the string on "(a)(nt)(a)(pre)(za)(tpe)(za)(pre)(a)(nt)(a)".

**Constraints:**

*   `1 <= text.length <= 1000`
*   `text` consists only of lowercase English characters.

## Solution

```java
public class Solution {
    public int longestDecomposition(String text) {
        int n = text.length();
        int l = 0;
        int r = n - 1;
        int len = 1;
        int ans = 0;
        String lft;
        String rit;
        boolean perfectSubstring = false;
        while (l + len <= r - len + 1) {
            lft = text.substring(l, l + len);
            rit = text.substring(r - len + 1, r + 1);
            if (lft.equals(rit)) {
                ans += 2;
                if (l + len == r - len + 1) {
                    perfectSubstring = true;
                    break;
                }
                l = l + len;
                r = r - len;
                len = 1;
            } else {
                len++;
            }
        }
        if (!perfectSubstring) {
            ans++;
        }
        return ans;
    }
}
```