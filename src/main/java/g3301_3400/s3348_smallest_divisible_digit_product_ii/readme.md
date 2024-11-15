[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3348\. Smallest Divisible Digit Product II

Hard

You are given a string `num` which represents a **positive** integer, and an integer `t`.

A number is called **zero-free** if _none_ of its digits are 0.

Return a string representing the **smallest** **zero-free** number greater than or equal to `num` such that the **product of its digits** is divisible by `t`. If no such number exists, return `"-1"`.

**Example 1:**

**Input:** num = "1234", t = 256

**Output:** "1488"

**Explanation:**

The smallest zero-free number that is greater than 1234 and has the product of its digits divisible by 256 is 1488, with the product of its digits equal to 256.

**Example 2:**

**Input:** num = "12355", t = 50

**Output:** "12355"

**Explanation:**

12355 is already zero-free and has the product of its digits divisible by 50, with the product of its digits equal to 150.

**Example 3:**

**Input:** num = "11111", t = 26

**Output:** "-1"

**Explanation:**

No number greater than 11111 has the product of its digits divisible by 26.

**Constraints:**

*   <code>2 <= num.length <= 2 * 10<sup>5</sup></code>
*   `num` consists only of digits in the range `['0', '9']`.
*   `num` does not contain leading zeros.
*   <code>1 <= t <= 10<sup>14</sup></code>

## Solution

```java
public class Solution {
    public String smallestNumber(String num, long t) {
        long tmp = t;
        for (int i = 9; i > 1; i--) {
            while (tmp % i == 0) {
                tmp /= i;
            }
        }
        if (tmp > 1) {
            return "-1";
        }

        char[] s = num.toCharArray();
        int n = s.length;
        long[] leftT = new long[n + 1];
        leftT[0] = t;
        int i0 = n - 1;
        for (int i = 0; i < n; i++) {
            if (s[i] == '0') {
                i0 = i;
                break;
            }
            leftT[i + 1] = leftT[i] / gcd(leftT[i], (long) s[i] - '0');
        }
        if (leftT[n] == 1) {
            return num;
        }
        for (int i = i0; i >= 0; i--) {
            while (++s[i] <= '9') {
                long tt = leftT[i] / gcd(leftT[i], (long) s[i] - '0');
                for (int j = n - 1; j > i; j--) {
                    if (tt == 1) {
                        s[j] = '1';
                        continue;
                    }
                    for (int k = 9; k > 1; k--) {
                        if (tt % k == 0) {
                            s[j] = (char) ('0' + k);
                            tt /= k;
                            break;
                        }
                    }
                }
                if (tt == 1) {
                    return new String(s);
                }
            }
        }
        StringBuilder ans = new StringBuilder();
        for (int i = 9; i > 1; i--) {
            while (t % i == 0) {
                ans.append((char) ('0' + i));
                t /= i;
            }
        }
        while (ans.length() <= n) {
            ans.append('1');
        }
        return ans.reverse().toString();
    }

    private long gcd(long a, long b) {
        while (a != 0) {
            long tmp = a;
            a = b % a;
            b = tmp;
        }
        return b;
    }
}
```