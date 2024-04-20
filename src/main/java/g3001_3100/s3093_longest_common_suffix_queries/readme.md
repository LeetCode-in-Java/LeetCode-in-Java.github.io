[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3093\. Longest Common Suffix Queries

Hard

You are given two arrays of strings `wordsContainer` and `wordsQuery`.

For each `wordsQuery[i]`, you need to find a string from `wordsContainer` that has the **longest common suffix** with `wordsQuery[i]`. If there are two or more strings in `wordsContainer` that share the longest common suffix, find the string that is the **smallest** in length. If there are two or more such strings that have the **same** smallest length, find the one that occurred **earlier** in `wordsContainer`.

Return _an array of integers_ `ans`_, where_ `ans[i]` _is the index of the string in_ `wordsContainer` _that has the **longest common suffix** with_ `wordsQuery[i]`_._

**Example 1:**

**Input:** wordsContainer = ["abcd","bcd","xbcd"], wordsQuery = ["cd","bcd","xyz"]

**Output:** [1,1,1]

**Explanation:**

Let's look at each `wordsQuery[i]` separately:

*   For `wordsQuery[0] = "cd"`, strings from `wordsContainer` that share the longest common suffix `"cd"` are at indices 0, 1, and 2. Among these, the answer is the string at index 1 because it has the shortest length of 3.
*   For `wordsQuery[1] = "bcd"`, strings from `wordsContainer` that share the longest common suffix `"bcd"` are at indices 0, 1, and 2. Among these, the answer is the string at index 1 because it has the shortest length of 3.
*   For `wordsQuery[2] = "xyz"`, there is no string from `wordsContainer` that shares a common suffix. Hence the longest common suffix is `""`, that is shared with strings at index 0, 1, and 2. Among these, the answer is the string at index 1 because it has the shortest length of 3.

**Example 2:**

**Input:** wordsContainer = ["abcdefgh","poiuygh","ghghgh"], wordsQuery = ["gh","acbfgh","acbfegh"]

**Output:** [2,0,2]

**Explanation:**

Let's look at each `wordsQuery[i]` separately:

*   For `wordsQuery[0] = "gh"`, strings from `wordsContainer` that share the longest common suffix `"gh"` are at indices 0, 1, and 2. Among these, the answer is the string at index 2 because it has the shortest length of 6.
*   For `wordsQuery[1] = "acbfgh"`, only the string at index 0 shares the longest common suffix `"fgh"`. Hence it is the answer, even though the string at index 2 is shorter.
*   For `wordsQuery[2] = "acbfegh"`, strings from `wordsContainer` that share the longest common suffix `"gh"` are at indices 0, 1, and 2. Among these, the answer is the string at index 2 because it has the shortest length of 6.

**Constraints:**

*   <code>1 <= wordsContainer.length, wordsQuery.length <= 10<sup>4</sup></code>
*   <code>1 <= wordsContainer[i].length <= 5 * 10<sup>3</sup></code>
*   <code>1 <= wordsQuery[i].length <= 5 * 10<sup>3</sup></code>
*   `wordsContainer[i]` consists only of lowercase English letters.
*   `wordsQuery[i]` consists only of lowercase English letters.
*   Sum of `wordsContainer[i].length` is at most <code>5 * 10<sup>5</sup></code>.
*   Sum of `wordsQuery[i].length` is at most <code>5 * 10<sup>5</sup></code>.

## Solution

```java
public class Solution {
    public int[] stringIndices(String[] wc, String[] wq) {
        int minLength = wc[0].length();
        int minIndex = 0;
        int n = wc.length;
        int m = wq.length;
        for (int i = 0; i < n; i++) {
            if (minLength > wc[i].length()) {
                minLength = wc[i].length();
                minIndex = i;
            }
        }
        Trie root = new Trie(minIndex);
        for (int i = 0; i < n; i++) {
            Trie curr = root;
            for (int j = wc[i].length() - 1; j >= 0; j--) {
                char ch = wc[i].charAt(j);
                if (curr.has(ch)) {
                    Trie next = curr.get(ch);
                    if (wc[next.index].length() > wc[i].length()) {
                        next.index = i;
                    }
                    curr = next;
                } else {
                    curr.put(ch, i);
                    curr = curr.get(ch);
                }
            }
        }
        int[] ans = new int[m];
        for (int i = 0; i < m; i++) {
            Trie curr = root;
            for (int j = wq[i].length() - 1; j >= 0; j--) {
                char ch = wq[i].charAt(j);
                if (curr.has(ch)) {
                    curr = curr.get(ch);
                } else {
                    break;
                }
            }
            ans[i] = curr.index;
        }

        return ans;
    }

    private static class Trie {
        Trie[] ch;
        int index;

        Trie(int index) {
            this.ch = new Trie[26];
            this.index = index;
        }

        Trie get(char ch) {
            return this.ch[ch - 'a'];
        }

        boolean has(char ch) {
            return this.ch[ch - 'a'] != null;
        }

        void put(char ch, int index) {
            this.ch[ch - 'a'] = new Trie(index);
        }
    }
}
```