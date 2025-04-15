[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3519\. Count Numbers with Non-Decreasing Digits

Hard

You are given two integers, `l` and `r`, represented as strings, and an integer `b`. Return the count of integers in the inclusive range `[l, r]` whose digits are in **non-decreasing** order when represented in base `b`.

An integer is considered to have **non-decreasing** digits if, when read from left to right (from the most significant digit to the least significant digit), each digit is greater than or equal to the previous one.

Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** l = "23", r = "28", b = 8

**Output:** 3

**Explanation:**

*   The numbers from 23 to 28 in base 8 are: 27, 30, 31, 32, 33, and 34.
*   Out of these, 27, 33, and 34 have non-decreasing digits. Hence, the output is 3.

**Example 2:**

**Input:** l = "2", r = "7", b = 2

**Output:** 2

**Explanation:**

*   The numbers from 2 to 7 in base 2 are: 10, 11, 100, 101, 110, and 111.
*   Out of these, 11 and 111 have non-decreasing digits. Hence, the output is 2.

**Constraints:**

*   `1 <= l.length <= r.length <= 100`
*   `2 <= b <= 10`
*   `l` and `r` consist only of digits.
*   The value represented by `l` is less than or equal to the value represented by `r`.
*   `l` and `r` do not contain leading zeros.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int countNumbers(String l, String r, int b) {
        long ans1 = find(r.toCharArray(), b);
        char[] start = subTractOne(l.toCharArray());
        long ans2 = find(start, b);
        return (int) ((ans1 - ans2) % 1000000007L);
    }

    private long find(char[] arr, int b) {
        int[] nums = convertNumToBase(arr, b);
        Long[][][] dp = new Long[nums.length][2][11];
        return solve(0, nums, 1, b, 0, dp) - 1;
    }

    private long solve(int i, int[] arr, int tight, int base, int last, Long[][][] dp) {
        if (i == arr.length) {
            return 1L;
        }
        if (dp[i][tight][last] != null) {
            return dp[i][tight][last];
        }
        int till = base - 1;
        if (tight == 1) {
            till = arr[i];
        }
        long ans = 0;
        for (int j = 0; j <= till; j++) {
            if (j >= last) {
                ans = (ans + solve(i + 1, arr, tight & (j == arr[i] ? 1 : 0), base, j, dp));
            }
        }
        dp[i][tight][last] = ans;
        return ans;
    }

    private char[] subTractOne(char[] arr) {
        int n = arr.length;
        int i = n - 1;
        while (i >= 0 && arr[i] == '0') {
            arr[i--] = '9';
        }
        int x = arr[i] - '0' - 1;
        arr[i] = (char) (x + '0');
        int j = 0;
        int idx = 0;
        while (j < n && arr[j] == '0') {
            j++;
        }
        char[] res = new char[n - j];
        for (int k = j; k < n; k++) {
            res[idx++] = arr[k];
        }
        return res;
    }

    private int[] convertNumToBase(char[] arr, int base) {
        int n = arr.length;
        int[] num = new int[n];
        int i = 0;
        while (i < n) {
            num[i] = arr[i++] - '0';
        }
        List<Integer> temp = new ArrayList<>();
        int len = n;
        while (len > 0) {
            int rem = 0;
            int[] next = new int[len];
            int newLen = 0;
            int j = 0;
            while (j < len) {
                long cur = rem * 10L + num[j];
                int q = (int) (cur / base);
                rem = (int) (cur % base);
                if (newLen > 0 || q != 0) {
                    next[newLen] = q;
                    newLen++;
                }
                j++;
            }
            temp.add(rem);
            num = next;
            len = newLen;
        }
        int[] res = new int[temp.size()];
        int k = 0;
        int size = temp.size();
        while (k < size) {
            res[k] = temp.get(size - 1 - k);
            k++;
        }
        return res;
    }
}
```