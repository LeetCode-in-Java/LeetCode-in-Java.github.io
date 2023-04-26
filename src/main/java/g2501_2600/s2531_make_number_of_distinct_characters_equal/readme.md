[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2531\. Make Number of Distinct Characters Equal

Medium

You are given two **0-indexed** strings `word1` and `word2`.

A **move** consists of choosing two indices `i` and `j` such that `0 <= i < word1.length` and `0 <= j < word2.length` and swapping `word1[i]` with `word2[j]`.

Return `true` _if it is possible to get the number of distinct characters in_ `word1` _and_ `word2` _to be equal with **exactly one** move._ Return `false` _otherwise_.

**Example 1:**

**Input:** word1 = "ac", word2 = "b"

**Output:** false

**Explanation:** Any pair of swaps would yield two distinct characters in the first string, and one in the second string.

**Example 2:**

**Input:** word1 = "abcc", word2 = "aab"

**Output:** true

**Explanation:** We swap index 2 of the first string with index 0 of the second string. The resulting strings are word1 = "abac" and word2 = "cab", which both have 3 distinct characters.

**Example 3:**

**Input:** word1 = "abcde", word2 = "fghij"

**Output:** true

**Explanation:** Both resulting strings will have 5 distinct characters, regardless of which indices we swap.

**Constraints:**

*   <code>1 <= word1.length, word2.length <= 10<sup>5</sup></code>
*   `word1` and `word2` consist of only lowercase English letters.

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    public boolean isItPossible(String word1, String word2) {
        int[] count1 = count(word1);
        int[] count2 = count(word2);
        int d = count1[26] - count2[26];
        int[] zip1 = zip(count1, count2);
        int[] zip2 = zip(count2, count1);
        for (int i = 0; i < 26; i++) {
            int d1 = zip1[i];
            if (d1 == -1) {
                continue;
            }
            for (int j = 0; j < 26; j++) {
                int d2 = zip2[j];
                if (d2 == -1) {
                    continue;
                }
                if (i == j) {
                    if (d == 0) {
                        return true;
                    }
                    continue;
                }
                if (d - d1 + d2 == 0) {
                    return true;
                }
            }
        }
        return false;
    }

    private int[] zip(int[] c1, int[] c2) {
        int[] zip = new int[26];
        for (int i = 0; i < 26; i++) {
            int d = 0;
            if (c1[i] == 0) {
                d = -1;
            } else {
                if (c2[i] == 0) {
                    d++;
                }
                if (c1[i] == 1) {
                    d++;
                }
            }
            zip[i] = d;
        }
        return zip;
    }

    private int[] count(String word) {
        int[] count = new int[27];
        int len = word.length();
        for (int i = 0; i < len; i++) {
            count[word.charAt(i) - 'a']++;
        }
        for (int i = 0; i < 26; i++) {
            if (count[i] > 0) {
                count[26]++;
            }
        }
        return count;
    }
}
```