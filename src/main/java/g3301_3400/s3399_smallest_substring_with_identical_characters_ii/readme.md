[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3399\. Smallest Substring With Identical Characters II

Hard

You are given a binary string `s` of length `n` and an integer `numOps`.

You are allowed to perform the following operation on `s` **at most** `numOps` times:

*   Select any index `i` (where `0 <= i < n`) and **flip** `s[i]`. If `s[i] == '1'`, change `s[i]` to `'0'` and vice versa.

You need to **minimize** the length of the **longest** substring of `s` such that all the characters in the substring are **identical**.

Return the **minimum** length after the operations.

**Example 1:**

**Input:** s = "000001", numOps = 1

**Output:** 2

**Explanation:**

By changing `s[2]` to `'1'`, `s` becomes `"001001"`. The longest substrings with identical characters are `s[0..1]` and `s[3..4]`.

**Example 2:**

**Input:** s = "0000", numOps = 2

**Output:** 1

**Explanation:**

By changing `s[0]` and `s[2]` to `'1'`, `s` becomes `"1010"`.

**Example 3:**

**Input:** s = "0101", numOps = 0

**Output:** 1

**Constraints:**

*   <code>1 <= n == s.length <= 10<sup>5</sup></code>
*   `s` consists only of `'0'` and `'1'`.
*   `0 <= numOps <= n`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int minLength(String s, int numOps) {
        byte[] b = s.getBytes();
        int flips1 = 0;
        int flips2 = 0;
        for (int i = 0; i < b.length; i++) {
            byte e1 = (byte) ((i % 2 == 0) ? '0' : '1');
            byte e2 = (byte) ((i % 2 == 0) ? '1' : '0');
            if (b[i] != e1) {
                flips1++;
            }
            if (b[i] != e2) {
                flips2++;
            }
        }
        int flips = Math.min(flips1, flips2);
        if (flips <= numOps) {
            return 1;
        }
        List<Integer> seg = new ArrayList<>();
        int count = 1;
        int max = 1;
        for (int i = 1; i < b.length; i++) {
            if (b[i] != b[i - 1]) {
                if (count != 1) {
                    seg.add(count);
                    max = Math.max(max, count);
                }
                count = 1;
            } else {
                count++;
            }
        }
        if (count != 1) {
            seg.add(count);
            max = Math.max(max, count);
        }
        int l = 2;
        int r = max;
        int res = max;
        while (l <= r) {
            int m = l + (r - l) / 2;
            if (check(m, seg, numOps)) {
                r = m - 1;
                res = m;
            } else {
                l = m + 1;
            }
        }
        return res;
    }

    private boolean check(int sz, List<Integer> seg, int ops) {
        for (int i : seg) {
            int x = i / (sz + 1);
            ops -= x;
            if (ops < 0) {
                return false;
            }
        }
        return true;
    }
}
```