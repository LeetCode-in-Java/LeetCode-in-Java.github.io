[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3445\. Maximum Difference Between Even and Odd Frequency II

Hard

You are given a string `s` and an integer `k`. Your task is to find the **maximum** difference between the frequency of **two** characters, `freq[a] - freq[b]`, in a **substring** `subs` of `s`, such that:

*   `subs` has a size of **at least** `k`.
*   Character `a` has an _odd frequency_ in `subs`.
*   Character `b` has an _even frequency_ in `subs`.

Return the **maximum** difference.

**Note** that `subs` can contain more than 2 **distinct** characters.

**Example 1:**

**Input:** s = "12233", k = 4

**Output:** \-1

**Explanation:**

For the substring `"12233"`, the frequency of `'1'` is 1 and the frequency of `'3'` is 2. The difference is `1 - 2 = -1`.

**Example 2:**

**Input:** s = "1122211", k = 3

**Output:** 1

**Explanation:**

For the substring `"11222"`, the frequency of `'2'` is 3 and the frequency of `'1'` is 2. The difference is `3 - 2 = 1`.

**Example 3:**

**Input:** s = "110", k = 3

**Output:** \-1

**Constraints:**

*   <code>3 <= s.length <= 3 * 10<sup>4</sup></code>
*   `s` consists only of digits `'0'` to `'4'`.
*   The input is generated that at least one substring has a character with an even frequency and a character with an odd frequency.
*   `1 <= k <= s.length`

## Solution

```java
import java.util.Arrays;

@SuppressWarnings("java:S135")
public class Solution {
    public int maxDifference(String s, int k) {
        int n = s.length();
        int[][] pre = new int[5][n];
        int[][] closestRight = new int[5][n];
        for (int x = 0; x < 5; x++) {
            Arrays.fill(closestRight[x], n);
            for (int i = 0; i < n; i++) {
                int num = s.charAt(i) - '0';
                pre[x][i] = (num == x) ? 1 : 0;
                if (i > 0) {
                    pre[x][i] += pre[x][i - 1];
                }
            }
            for (int i = n - 1; i >= 0; i--) {
                int num = s.charAt(i) - '0';
                if (i < n - 1) {
                    closestRight[x][i] = closestRight[x][i + 1];
                }
                if (num == x) {
                    closestRight[x][i] = i;
                }
            }
        }
        int ans = Integer.MIN_VALUE;
        for (int a = 0; a <= 4; a++) {
            for (int b = 0; b <= 4; b++) {
                if (a != b) {
                    ans = Math.max(ans, go(k, a, b, pre, closestRight, n));
                }
            }
        }
        return ans;
    }

    private int go(int k, int odd, int even, int[][] pre, int[][] closestRight, int n) {
        int[][][] suf = new int[2][2][n];
        for (int[][] arr2D : suf) {
            for (int[] arr : arr2D) {
                Arrays.fill(arr, Integer.MIN_VALUE);
            }
        }
        for (int endIdx = 0; endIdx < n; endIdx++) {
            int oddParity = pre[odd][endIdx] % 2;
            int evenParity = pre[even][endIdx] % 2;
            if (pre[odd][endIdx] > 0 && pre[even][endIdx] > 0) {
                suf[oddParity][evenParity][endIdx] = pre[odd][endIdx] - pre[even][endIdx];
            }
        }
        for (int oddParity = 0; oddParity < 2; oddParity++) {
            for (int evenParity = 0; evenParity < 2; evenParity++) {
                for (int endIdx = n - 2; endIdx >= 0; endIdx--) {
                    suf[oddParity][evenParity][endIdx] =
                            Math.max(
                                    suf[oddParity][evenParity][endIdx],
                                    suf[oddParity][evenParity][endIdx + 1]);
                }
            }
        }
        int ans = Integer.MIN_VALUE;
        for (int startIdx = 0; startIdx < n; startIdx++) {
            int minEndIdx = startIdx + k - 1;
            if (minEndIdx >= n) {
                break;
            }
            int oddBelowI = (startIdx == 0 ? 0 : pre[odd][startIdx - 1]);
            int evenBelowI = (startIdx == 0 ? 0 : pre[even][startIdx - 1]);
            int goodOddParity = (oddBelowI + 1) % 2;
            int goodEvenParity = evenBelowI % 2;
            int query =
                    Math.max(
                            Math.max(startIdx + k - 1, closestRight[odd][startIdx]),
                            closestRight[even][startIdx]);
            if (query >= n) {
                continue;
            }
            int val = suf[goodOddParity][goodEvenParity][query];
            if (val == Integer.MIN_VALUE) {
                continue;
            }
            ans = Math.max(ans, val - oddBelowI + evenBelowI);
        }
        return ans;
    }
}
```