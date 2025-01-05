[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3398\. Smallest Substring With Identical Characters I

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

*   `1 <= n == s.length <= 1000`
*   `s` consists only of `'0'` and `'1'`.
*   `0 <= numOps <= n`

## Solution

```java
public class Solution {
    public int minLength(String s, int ops) {
        char[] arr2 = s.toCharArray();
        int q = '0';
        int w = '1';
        int p1 = ops;
        int p2 = ops;
        for (int i = 0; i < s.length(); i++) {
            if (arr2[i] != q) {
                p1--;
            }
            if (arr2[i] != w) {
                p2--;
            }
            if (q == '0') {
                q = '1';
            } else {
                q = '0';
            }
            if (w == '0') {
                w = '1';
            } else {
                w = '0';
            }
        }
        if (p1 >= 0 || p2 >= 0) {
            return 1;
        }
        int low = 2;
        int high = s.length();
        int ans = 0;
        int n = s.length();
        while (low <= high) {
            int mid = (low + high) / 2;
            char[] arr = s.toCharArray();
            int p = ops;
            int c = 1;
            for (int i = 1; i < n; i++) {
                if (arr[i] == arr[i - 1]) {
                    c++;
                } else {
                    c = 1;
                }
                if (c > mid) {
                    if (arr[i - 1] == '0') {
                        arr[i - 1] = '1';
                    } else {
                        arr[i - 1] = '0';
                    }
                    p--;
                    c = 0;
                }
            }
            if (p < 0) {
                low = mid + 1;
            } else {
                ans = mid;
                high = mid - 1;
            }
        }
        return ans;
    }
}
```