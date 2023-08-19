[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2559\. Count Vowel Strings in Ranges

Medium

You are given a **0-indexed** array of strings `words` and a 2D array of integers `queries`.

Each query <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> asks us to find the number of strings present in the range <code>l<sub>i</sub></code> to <code>r<sub>i</sub></code> (both **inclusive**) of `words` that start and end with a vowel.

Return _an array_ `ans` _of size_ `queries.length`_, where_ `ans[i]` _is the answer to the_ `i`<sup>th</sup> _query_.

**Note** that the vowel letters are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

**Example 1:**

**Input:** words = ["aba","bcb","ece","aa","e"], queries = \[\[0,2],[1,4],[1,1]]

**Output:** [2,3,0]

**Explanation:** The strings starting and ending with a vowel are "aba", "ece", "aa" and "e". 

The answer to the query [0,2] is 2 (strings "aba" and "ece"). 

to query [1,4] is 3 (strings "ece", "aa", "e"). 

to query [1,1] is 0. 

We return [2,3,0].

**Example 2:**

**Input:** words = ["a","e","i"], queries = \[\[0,2],[0,1],[2,2]]

**Output:** [3,2,1]

**Explanation:** Every string satisfies the conditions, so we return [3,2,1].

**Constraints:**

*   <code>1 <= words.length <= 10<sup>5</sup></code>
*   `1 <= words[i].length <= 40`
*   `words[i]` consists only of lowercase English letters.
*   <code>sum(words[i].length) <= 3 * 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>0 <= l<sub>i</sub> <= r<sub>i</sub> < words.length</code>

## Solution

```java
public class Solution {
    private boolean validWord(String s) {
        char cStart = s.charAt(0);
        char cEnd = s.charAt(s.length() - 1);
        boolean flag1 =
                cStart == 'a' || cStart == 'e' || cStart == 'i' || cStart == 'o' || cStart == 'u';
        boolean flag2 = cEnd == 'a' || cEnd == 'e' || cEnd == 'i' || cEnd == 'o' || cEnd == 'u';
        return flag1 && flag2;
    }

    public int[] vowelStrings(String[] words, int[][] queries) {
        int[] prefixArr = new int[words.length];
        prefixArr[0] = validWord(words[0]) ? 1 : 0;
        for (int i = 1; i < words.length; ++i) {
            if (validWord(words[i])) {
                prefixArr[i] = prefixArr[i - 1] + 1;
            } else {
                prefixArr[i] = prefixArr[i - 1];
            }
        }
        int[] res = new int[queries.length];
        for (int i = 0; i < queries.length; ++i) {
            int upperBound = queries[i][1];
            int lowerBound = queries[i][0];
            int val =
                    (lowerBound == 0)
                            ? prefixArr[upperBound]
                            : prefixArr[upperBound] - prefixArr[lowerBound - 1];
            res[i] = val;
        }
        return res;
    }
}
```