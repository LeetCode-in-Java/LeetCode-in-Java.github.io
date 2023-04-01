[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1297\. Maximum Number of Occurrences of a Substring

Medium

Given a string `s`, return the maximum number of ocurrences of **any** substring under the following rules:

*   The number of unique characters in the substring must be less than or equal to `maxLetters`.
*   The substring size must be between `minSize` and `maxSize` inclusive.

**Example 1:**

**Input:** s = "aababcaab", maxLetters = 2, minSize = 3, maxSize = 4

**Output:** 2

**Explanation:** Substring "aab" has 2 ocurrences in the original string.

It satisfies the conditions, 2 unique letters and size 3 (between minSize and maxSize).

**Example 2:**

**Input:** s = "aaaa", maxLetters = 1, minSize = 3, maxSize = 3

**Output:** 2

**Explanation:** Substring "aaa" occur 2 times in the string. It can overlap.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `1 <= maxLetters <= 26`
*   `1 <= minSize <= maxSize <= min(26, s.length)`
*   `s` consists of only lowercase English letters.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

@SuppressWarnings("java:S1172")
public class Solution {
    public int maxFreq(String s, int max, int minSize, int maxSize) {
        // the map of occurrences
        Map<String, Integer> sub2Count = new HashMap<>();
        // sliding window indices
        int lo = 0;
        int hi = minSize - 1;
        int maxCount = 0;
        // unique letters counter
        char[] uniq = new char[26];
        int uniqCount = 0;
        // initial window calculation - `hi` is excluded here!
        for (char ch : s.substring(lo, hi).toCharArray()) {
            uniq[ch - 'a'] += 1;
            if (uniq[ch - 'a'] == 1) {
                uniqCount++;
            }
        }
        while (hi < s.length()) {
            // handle increment of hi
            char hiCh = s.charAt(hi);
            uniq[hiCh - 'a'] += 1;
            if (uniq[hiCh - 'a'] == 1) {
                uniqCount++;
            }
            ++hi;
            // add the substring to the map of occurences
            String sub = s.substring(lo, hi);
            if (uniqCount <= max) {
                int count = 1;
                if (sub2Count.containsKey(sub)) {
                    count += sub2Count.get(sub);
                }
                sub2Count.put(sub, count);
                maxCount = Math.max(maxCount, count);
            }
            // handle increment of lo
            char loCh = s.charAt(lo);
            uniq[loCh - 'a'] -= 1;
            if (uniq[loCh - 'a'] == 0) {
                uniqCount--;
            }
            ++lo;
        }
        return maxCount;
    }
}
```