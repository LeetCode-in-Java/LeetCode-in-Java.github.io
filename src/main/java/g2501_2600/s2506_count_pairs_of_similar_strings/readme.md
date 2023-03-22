[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2506\. Count Pairs Of Similar Strings

Easy

You are given a **0-indexed** string array `words`.

Two strings are **similar** if they consist of the same characters.

*   For example, `"abca"` and `"cba"` are similar since both consist of characters `'a'`, `'b'`, and `'c'`.
*   However, `"abacba"` and `"bcfd"` are not similar since they do not consist of the same characters.

Return _the number of pairs_ `(i, j)` _such that_ `0 <= i < j <= word.length - 1` _and the two strings_ `words[i]` _and_ `words[j]` _are similar_.

**Example 1:**

**Input:** words = ["aba","aabb","abcd","bac","aabc"]

**Output:** 2

**Explanation:** There are 2 pairs that satisfy the conditions: 

- i = 0 and j = 1 : both words[0] and words[1] only consist of characters 'a' and 'b'. 

- i = 3 and j = 4 : both words[3] and words[4] only consist of characters 'a', 'b', and 'c'.

**Example 2:**

**Input:** words = ["aabb","ab","ba"]

**Output:** 3

**Explanation:** There are 3 pairs that satisfy the conditions: 

- i = 0 and j = 1 : both words[0] and words[1] only consist of characters 'a' and 'b'. 

- i = 0 and j = 2 : both words[0] and words[2] only consist of characters 'a' and 'b'. 

- i = 1 and j = 2 : both words[1] and words[2] only consist of characters 'a' and 'b'.

**Example 3:**

**Input:** words = ["nba","cba","dba"]

**Output:** 0

**Explanation:** Since there does not exist any pair that satisfies the conditions, we return 0.

**Constraints:**

*   `1 <= words.length <= 100`
*   `1 <= words[i].length <= 100`
*   `words[i]` consist of only lowercase English letters.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int similarPairs(String[] words) {
        int len = words.length;
        if (len == 1) {
            return 0;
        }
        byte[][] alPh = new byte[len][26];
        Map<String, Integer> map = new HashMap<>();
        for (int i = 0; i < len; i++) {
            String word = words[i];
            for (char c : word.toCharArray()) {
                int idx = c - 'a';
                if (alPh[i][idx] == 0) {
                    alPh[i][idx]++;
                }
            }
            String s = new String(alPh[i]);
            if (map.containsKey(s)) {
                map.put(s, map.get(s) + 1);
            } else {
                map.put(s, 1);
            }
        }
        int pairs = 0;
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            int freq = entry.getValue();
            if (freq > 1) {
                pairs += (freq * (freq - 1)) / 2;
            }
        }
        return pairs;
    }
}
```