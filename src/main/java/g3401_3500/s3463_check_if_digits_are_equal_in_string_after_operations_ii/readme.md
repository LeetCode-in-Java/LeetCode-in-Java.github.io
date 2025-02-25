[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3463\. Check If Digits Are Equal in String After Operations II

Hard

You are given a string `s` consisting of digits. Perform the following operation repeatedly until the string has **exactly** two digits:

*   For each pair of consecutive digits in `s`, starting from the first digit, calculate a new digit as the sum of the two digits **modulo** 10.
*   Replace `s` with the sequence of newly calculated digits, _maintaining the order_ in which they are computed.

Return `true` if the final two digits in `s` are the **same**; otherwise, return `false`.

**Example 1:**

**Input:** s = "3902"

**Output:** true

**Explanation:**

*   Initially, `s = "3902"`
*   First operation:
    *   `(s[0] + s[1]) % 10 = (3 + 9) % 10 = 2`
    *   `(s[1] + s[2]) % 10 = (9 + 0) % 10 = 9`
    *   `(s[2] + s[3]) % 10 = (0 + 2) % 10 = 2`
    *   `s` becomes `"292"`
*   Second operation:
    *   `(s[0] + s[1]) % 10 = (2 + 9) % 10 = 1`
    *   `(s[1] + s[2]) % 10 = (9 + 2) % 10 = 1`
    *   `s` becomes `"11"`
*   Since the digits in `"11"` are the same, the output is `true`.

**Example 2:**

**Input:** s = "34789"

**Output:** false

**Explanation:**

*   Initially, `s = "34789"`.
*   After the first operation, `s = "7157"`.
*   After the second operation, `s = "862"`.
*   After the third operation, `s = "48"`.
*   Since `'4' != '8'`, the output is `false`.

**Constraints:**

*   <code>3 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only digits.

## Solution

```java
public class Solution {
    private int powMod10(int a, int n) {
        int x = 1;
        while (n >= 1) {
            if (n % 2 == 1) {
                x = (x * a) % 10;
            }
            a = (a * a) % 10;
            n /= 2;
        }
        return x;
    }

    private int[] f(int n) {
        int[] ns = new int[n + 1];
        int[] n2 = new int[n + 1];
        int[] n5 = new int[n + 1];
        ns[0] = 1;
        for (int i = 1; i <= n; ++i) {
            int m = i;
            n2[i] = n2[i - 1];
            n5[i] = n5[i - 1];
            while (m % 2 == 0) {
                m /= 2;
                n2[i]++;
            }
            while (m % 5 == 0) {
                m /= 5;
                n5[i]++;
            }
            ns[i] = (ns[i - 1] * m) % 10;
        }
        int[] inv = new int[10];
        for (int i = 1; i < 10; ++i) {
            for (int j = 0; j < 10; ++j) {
                if (i * j % 10 == 1) {
                    inv[i] = j;
                }
            }
        }
        int[] xs = new int[n + 1];
        for (int k = 0; k <= n; ++k) {
            int a = 0;
            int s2 = n2[n] - n2[n - k] - n2[k];
            int s5 = n5[n] - n5[n - k] - n5[k];
            if (s2 == 0 || s5 == 0) {
                a = (ns[n] * inv[ns[n - k]] * inv[ns[k]] * powMod10(2, s2) * powMod10(5, s5)) % 10;
            }
            xs[k] = a;
        }
        return xs;
    }

    public boolean hasSameDigits(String s) {
        int n = s.length();
        int[] xs = f(n - 2);
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) {
            arr[i] = s.charAt(i) - '0';
        }
        int num1 = 0;
        int num2 = 0;
        for (int i = 0; i < n - 1; i++) {
            num1 = (num1 + xs[i] * arr[i]) % 10;
            num2 = (num2 + xs[i] * arr[i + 1]) % 10;
        }
        return num1 == num2;
    }
}
```