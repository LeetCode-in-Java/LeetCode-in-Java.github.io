[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2380\. Time Needed to Rearrange a Binary String

Medium

You are given a binary string `s`. In one second, **all** occurrences of `"01"` are **simultaneously** replaced with `"10"`. This process **repeats** until no occurrences of `"01"` exist.

Return _the number of seconds needed to complete this process._

**Example 1:**

**Input:** s = "0110101"

**Output:** 4

**Explanation:**

After one second, s becomes "1011010".

After another second, s becomes "1101100".

After the third second, s becomes "1110100".

After the fourth second, s becomes "1111000".

No occurrence of "01" exists any longer, and the process needed 4 seconds to complete, so we return 4.

**Example 2:**

**Input:** s = "11100"

**Output:** 0

**Explanation:** No occurrence of "01" exists in s, and the processes needed 0 seconds to complete, so we return 0.

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s[i]` is either `'0'` or `'1'`.

**Follow up:**

Can you solve this problem in O(n) time complexity?

## Solution

```java
public class Solution {
    public int secondsToRemoveOccurrences(String s) {
        int lastOne = -1;
        int result = 0;
        int prevResult = 0;
        int curResult = 0;
        int countOne = 0;
        int countZero = 0;
        int diff;
        int pTarget;
        int pWait;
        int cTarget;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == '0') {
                ++countZero;
                continue;
            }
            ++countOne;
            diff = i - lastOne - 1;
            prevResult = curResult;
            cTarget = countOne - 1;
            pTarget = cTarget - 1;
            pWait = prevResult - (lastOne - pTarget);
            if (diff > pWait) {
                curResult = countZero;
            } else {
                curResult = countZero == 0 ? 0 : pWait - diff + 1 + countZero;
            }
            result = curResult;
            lastOne = i;
        }
        return result;
    }
}
```