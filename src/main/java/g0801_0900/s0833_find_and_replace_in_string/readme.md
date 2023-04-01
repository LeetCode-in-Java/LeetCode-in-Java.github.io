[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 833\. Find And Replace in String

Medium

You are given a **0-indexed** string `s` that you must perform `k` replacement operations on. The replacement operations are given as three **0-indexed** parallel arrays, `indices`, `sources`, and `targets`, all of length `k`.

To complete the <code>i<sup>th</sup></code> replacement operation:

1.  Check if the **substring** `sources[i]` occurs at index `indices[i]` in the **original string** `s`.
2.  If it does not occur, **do nothing**.
3.  Otherwise if it does occur, **replace** that substring with `targets[i]`.

For example, if `s = "abcd"`, `indices[i] = 0`, `sources[i] = "ab"`, and `targets[i] = "eee"`, then the result of this replacement will be `"eeecd"`.

All replacement operations must occur **simultaneously**, meaning the replacement operations should not affect the indexing of each other. The testcases will be generated such that the replacements will **not overlap**.

*   For example, a testcase with `s = "abc"`, `indices = [0, 1]`, and `sources = ["ab","bc"]` will not be generated because the `"ab"` and `"bc"` replacements overlap.

Return _the **resulting string** after performing all replacement operations on_ `s`.

A **substring** is a contiguous sequence of characters in a string.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/12/833-ex1.png)

**Input:** s = "abcd", indices = [0, 2], sources = ["a", "cd"], targets = ["eee", "ffff"]

**Output:** "eeebffff"

**Explanation:** "a" occurs at index 0 in s, so we replace it with "eee". "cd" occurs at index 2 in s, so we replace it with "ffff".

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/12/833-ex2-1.png)

**Input:** s = "abcd", indices = [0, 2], sources = ["ab","ec"], targets = ["eee","ffff"]

**Output:** "eeecd"

**Explanation:** "ab" occurs at index 0 in s, so we replace it with "eee". "ec" does not occur at index 2 in s, so we do nothing.

**Constraints:**

*   `1 <= s.length <= 1000`
*   `k == indices.length == sources.length == targets.length`
*   `1 <= k <= 100`
*   `0 <= indexes[i] < s.length`
*   `1 <= sources[i].length, targets[i].length <= 50`
*   `s` consists of only lowercase English letters.
*   `sources[i]` and `targets[i]` consist of only lowercase English letters.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public String findReplaceString(String s, int[] indices, String[] sources, String[] targets) {
        StringBuilder sb = new StringBuilder();
        Map<Integer, Integer> stringIndexToKIndex = new HashMap<>();
        for (int i = 0; i < indices.length; ++i) {
            stringIndexToKIndex.put(indices[i], i);
        }
        int indexIntoS = 0;
        while (indexIntoS < s.length()) {
            if (stringIndexToKIndex.containsKey(indexIntoS)) {
                String substringInSources = sources[stringIndexToKIndex.get(indexIntoS)];
                if (indexIntoS + substringInSources.length() <= s.length()) {
                    String substringInS =
                            s.substring(indexIntoS, indexIntoS + substringInSources.length());
                    if (substringInS.equals(substringInSources)) {
                        sb.append(targets[stringIndexToKIndex.get(indexIntoS)]);
                        indexIntoS += substringInS.length() - 1;
                    } else {
                        sb.append(s.charAt(indexIntoS));
                    }
                } else {
                    sb.append(s.charAt(indexIntoS));
                }
            } else {
                sb.append(s.charAt(indexIntoS));
            }
            indexIntoS++;
        }
        return sb.toString();
    }
}
```