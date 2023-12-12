[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2827\. Number of Beautiful Integers in the Range

Hard

You are given positive integers `low`, `high`, and `k`.

A number is **beautiful** if it meets both of the following conditions:

*   The count of even digits in the number is equal to the count of odd digits.
*   The number is divisible by `k`.

Return _the number of beautiful integers in the range_ `[low, high]`.

**Example 1:**

**Input:** low = 10, high = 20, k = 3

**Output:** 2

**Explanation:** There are 2 beautiful integers in the given range: [12,18]. 
- 12 is beautiful because it contains 1 odd digit and 1 even digit, and is divisible by k = 3. 
- 18 is beautiful because it contains 1 odd digit and 1 even digit, and is divisible by k = 3. Additionally we can see that: 
- 16 is not beautiful because it is not divisible by k = 3. 
- 15 is not beautiful because it does not contain equal counts even and odd digits. It can be shown that there are only 2 beautiful integers in the given range.

**Example 2:**

**Input:** low = 1, high = 10, k = 1

**Output:** 1

**Explanation:** There is 1 beautiful integer in the given range: [10]. 
- 10 is beautiful because it contains 1 odd digit and 1 even digit, and is divisible by k = 1. It can be shown that there is only 1 beautiful integer in the given range.

**Example 3:**

**Input:** low = 5, high = 5, k = 2

**Output:** 0

**Explanation:** There are 0 beautiful integers in the given range. 
- 5 is not beautiful because it is not divisible by k = 2 and it does not contain equal even and odd digits.

**Constraints:**

*   <code>0 < low <= high <= 10<sup>9</sup></code>
*   `0 < k <= 20`

## Solution

```java
import java.util.Arrays;

@SuppressWarnings("java:S107")
public class Solution {
    private int[][][][][] dp;
    private int maxLength;

    public int numberOfBeautifulIntegers(int low, int high, int k) {
        String num1 = String.valueOf(low);
        String num2 = String.valueOf(high);
        maxLength = Math.max(num1.length(), num2.length());
        dp = new int[4][maxLength][maxLength][maxLength][k];
        for (int[][][][] a : dp) {
            for (int[][][] b : a) {
                for (int[][] c : b) {
                    for (int[] d : c) {
                        Arrays.fill(d, -1);
                    }
                }
            }
        }
        return dp(num1, num2, 0, 3, 0, 0, 0, 0, k);
    }

    private int dp(
            String low, String high, int i, int mode, int odd, int even, int num, int rem, int k) {
        if (i == maxLength) {
            return num % k == 0 && odd == even ? 1 : 0;
        }
        if (dp[mode][i][odd][even][rem] != -1) {
            return dp[mode][i][odd][even][rem];
        }
        int res = 0;
        boolean lowLimit = mode % 2 == 1;
        boolean highLimit = mode / 2 == 1;
        int start = 0;
        int end = 9;
        if (lowLimit) {
            start = digitAt(low, i);
        }
        if (highLimit) {
            end = digitAt(high, i);
        }
        for (int j = start; j <= end; j++) {
            int newMode = 0;
            if (j == start && lowLimit) {
                newMode += 1;
            }
            if (j == end && highLimit) {
                newMode += 2;
            }
            int newEven = even;
            if (num != 0 || j != 0) {
                newEven += j % 2 == 0 ? 1 : 0;
            }
            int newOdd = odd + (j % 2 == 1 ? 1 : 0);
            res +=
                    dp(
                            low,
                            high,
                            i + 1,
                            newMode,
                            newOdd,
                            newEven,
                            num * 10 + j,
                            (num * 10 + j) % k,
                            k);
        }
        dp[mode][i][odd][even][rem] = res;
        return res;
    }

    private int digitAt(String num, int i) {
        int index = num.length() - maxLength + i;
        return index < 0 ? 0 : num.charAt(index) - '0';
    }
}
```