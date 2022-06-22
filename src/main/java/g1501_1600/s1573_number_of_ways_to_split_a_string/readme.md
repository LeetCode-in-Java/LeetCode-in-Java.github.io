## 1573\. Number of Ways to Split a String

Medium

Given a binary string `s`, you can split `s` into 3 **non-empty** strings `s1`, `s2`, and `s3` where `s1 + s2 + s3 = s`.

Return the number of ways `s` can be split such that the number of ones is the same in `s1`, `s2`, and `s3`. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "10101"

**Output:** 4

**Explanation:** There are four ways to split s in 3 parts where each part contain the same number of letters '1'. 

"1\|010\|1" 

"1\|01\|01" 

"10\|10\|1" 

"10\|1\|01"

**Example 2:**

**Input:** s = "1001"

**Output:** 0

**Example 3:**

**Input:** s = "0000"

**Output:** 3

**Explanation:** There are three ways to split s in 3 parts. 

"0\|0\|00" 

"0\|00\|0" 

"00\|0\|0"

**Constraints:**

*   <code>3 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.

## Solution

```java
public class Solution {
    public int numWays(String s) {
        long totalOnesCount = 0;
        long mod = 1000000007;
        long waysOfFirstString = 0;
        long waysOfSecondString = 0;
        long onesCount = 0;
        long n = s.length();

        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '1') {
                totalOnesCount += 1;
            }
        }

        if (totalOnesCount % 3 != 0) {
            return 0;
        }

        long onesFirstPart = totalOnesCount / 3;
        long onesSecondPart = onesFirstPart * 2;

        if (totalOnesCount == 0) {
            return (int) ((n - 1) * (n - 2) / 2 % mod);
        }

        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '1') {
                onesCount += 1;
            }
            if (onesCount == onesFirstPart) {
                waysOfFirstString += 1;
            } else if (onesCount == onesSecondPart) {
                waysOfSecondString += 1;
            } else if (onesCount > onesSecondPart) {
                break;
            }
        }

        return (int) ((waysOfFirstString * waysOfSecondString) % mod);
    }
}
```