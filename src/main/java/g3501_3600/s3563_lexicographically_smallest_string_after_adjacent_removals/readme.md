[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3563\. Lexicographically Smallest String After Adjacent Removals

Hard

You are given a string `s` consisting of lowercase English letters.

You can perform the following operation any number of times (including zero):

*   Remove **any** pair of **adjacent** characters in the string that are **consecutive** in the alphabet, in either order (e.g., `'a'` and `'b'`, or `'b'` and `'a'`).
*   Shift the remaining characters to the left to fill the gap.

Return the **lexicographically smallest** string that can be obtained after performing the operations optimally.

**Note:** Consider the alphabet as circular, thus `'a'` and `'z'` are consecutive.

**Example 1:**

**Input:** s = "abc"

**Output:** "a"

**Explanation:**

*   Remove `"bc"` from the string, leaving `"a"` as the remaining string.
*   No further operations are possible. Thus, the lexicographically smallest string after all possible removals is `"a"`.

**Example 2:**

**Input:** s = "bcda"

**Output:** ""

**Explanation:**

*   Remove `"cd"` from the string, leaving `"ba"` as the remaining string.
*   Remove `"ba"` from the string, leaving `""` as the remaining string.
*   No further operations are possible. Thus, the lexicographically smallest string after all possible removals is `""`.

**Example 3:**

**Input:** s = "zdce"

**Output:** "zdce"

**Explanation:**

*   Remove `"dc"` from the string, leaving `"ze"` as the remaining string.
*   No further operations are possible on `"ze"`.
*   However, since `"zdce"` is lexicographically smaller than `"ze"`, the smallest string after all possible removals is `"zdce"`.

**Constraints:**

*   `1 <= s.length <= 250`
*   `s` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    private boolean checkPair(char char1, char char2) {
        int diffVal = Math.abs(char1 - char2);
        return diffVal == 1 || (char1 == 'a' && char2 == 'z') || (char1 == 'z' && char2 == 'a');
    }

    public String lexicographicallySmallestString(String sIn) {
        int nVal = sIn.length();
        if (nVal == 0) {
            return "";
        }
        boolean[][] remTable = new boolean[nVal][nVal];
        for (int len = 2; len <= nVal; len += 2) {
            for (int idx = 0; idx <= nVal - len; idx++) {
                int j = idx + len - 1;
                if (checkPair(sIn.charAt(idx), sIn.charAt(j))) {
                    if (len == 2) {
                        remTable[idx][j] = true;
                    } else {
                        if (remTable[idx + 1][j - 1]) {
                            remTable[idx][j] = true;
                        }
                    }
                }
                if (remTable[idx][j]) {
                    continue;
                }
                for (int pSplit = idx + 1; pSplit < j; pSplit += 2) {
                    if (remTable[idx][pSplit] && remTable[pSplit + 1][j]) {
                        remTable[idx][j] = true;
                        break;
                    }
                }
            }
        }
        String[] dpArr = new String[nVal + 1];
        dpArr[nVal] = "";
        for (int idx = nVal - 1; idx >= 0; idx--) {
            dpArr[idx] = sIn.charAt(idx) + dpArr[idx + 1];
            for (int kMatch = idx + 1; kMatch < nVal; kMatch++) {
                if (checkPair(sIn.charAt(idx), sIn.charAt(kMatch))) {
                    boolean middleVanishes;
                    if (kMatch - 1 < idx + 1) {
                        middleVanishes = true;
                    } else {
                        middleVanishes = remTable[idx + 1][kMatch - 1];
                    }
                    if (middleVanishes) {
                        String candidate = dpArr[kMatch + 1];
                        if (candidate.compareTo(dpArr[idx]) < 0) {
                            dpArr[idx] = candidate;
                        }
                    }
                }
            }
        }
        return dpArr[0];
    }
}
```