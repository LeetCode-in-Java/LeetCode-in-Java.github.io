## 541\. Reverse String II

Easy

Given a string `s` and an integer `k`, reverse the first `k` characters for every `2k` characters counting from the start of the string.

If there are fewer than `k` characters left, reverse all of them. If there are less than `2k` but greater than or equal to `k` characters, then reverse the first `k` characters and left the other as original.

**Example 1:**

**Input:** s = "abcdefg", k = 2

**Output:** "bacdfeg"

**Example 2:**

**Input:** s = "abcd", k = 2

**Output:** "bacd"

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of only lowercase English letters.
*   <code>1 <= k <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public String reverseStr(String s, int k) {
        StringBuilder res = new StringBuilder();
        int p1 = 0;
        int p2 = 2 * k - 1;
        if (s.length() < k) {
            res.append(reverse(s));
            return res.toString();
        }
        while (p1 < s.length()) {
            if (s.length() < p1 + k) {
                res.append(reverse(s.substring(p1)));
            } else {
                res.append(reverse(s.substring(p1, p1 + k)));
                if (s.length() < p2 + 1) {
                    res.append(s.substring(p1 + k));
                } else {
                    res.append(s, p1 + k, p2 + 1);
                }
            }
            p1 = p1 + 2 * k;
            p2 = p2 + 2 * k;
        }
        return res.toString();
    }

    private String reverse(String s) {
        StringBuilder sb = new StringBuilder();
        sb.append(s);
        sb.reverse();
        return sb.toString();
    }
}
```