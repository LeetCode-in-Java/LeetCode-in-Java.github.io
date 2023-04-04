[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1416\. Restore The Array

Hard

A program was supposed to print an array of integers. The program forgot to print whitespaces and the array is printed as a string of digits `s` and all we know is that all integers in the array were in the range `[1, k]` and there are no leading zeros in the array.

Given the string `s` and the integer `k`, return _the number of the possible arrays that can be printed as_ `s` _using the mentioned program_. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "1000", k = 10000

**Output:** 1

**Explanation:** The only possible array is [1000]

**Example 2:**

**Input:** s = "1000", k = 10

**Output:** 0

**Explanation:** There cannot be an array that was printed this way and has all integer >= 1 and <= 10.

**Example 3:**

**Input:** s = "1317", k = 2000

**Output:** 8

**Explanation:** Possible arrays are [1317],[131,7],[13,17],[1,317],[13,1,7],[1,31,7],[1,3,17],[1,3,1,7]

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only digits and does not contain leading zeros.
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int numberOfArrays(String s, int k) {
        int kMod = 1000000007;
        int n = s.length();
        int[] dp = new int[n];
        if (s.charAt(n - 1) != '0') {
            dp[n - 1] = 1;
        }
        for (int i = n - 2; i >= 0; i--) {
            if (s.charAt(i) == '0') {
                continue;
            }
            long temp = 0;
            int j = i;
            while (j < n && temp * 10 + s.charAt(j) - '0' <= k) {
                temp = temp * 10 + s.charAt(j) - '0';
                if (j == n - 1) {
                    dp[i] += 1;
                } else {
                    dp[i] += dp[j + 1];
                }
                dp[i] %= kMod;
                j++;
            }
        }
        return dp[0];
    }
}
```