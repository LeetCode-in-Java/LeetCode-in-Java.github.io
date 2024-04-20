[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3076\. Shortest Uncommon Substring in an Array

Medium

You are given an array `arr` of size `n` consisting of **non-empty** strings.

Find a string array `answer` of size `n` such that:

*   `answer[i]` is the **shortest** substring of `arr[i]` that does **not** occur as a substring in any other string in `arr`. If multiple such substrings exist, `answer[i]` should be the lexicographically smallest. And if no such substring exists, `answer[i]` should be an empty string.

Return _the array_ `answer`.

**Example 1:**

**Input:** arr = ["cab","ad","bad","c"]

**Output:** ["ab","","ba",""]

**Explanation:** We have the following: 
- For the string "cab", the shortest substring that does not occur in any other string is either "ca" or "ab", we choose the lexicographically smaller substring, which is "ab". 
- For the string "ad", there is no substring that does not occur in any other string. 
- For the string "bad", the shortest substring that does not occur in any other string is "ba". 
- For the string "c", there is no substring that does not occur in any other string.

**Example 2:**

**Input:** arr = ["abc","bcd","abcd"]

**Output:** ["","","abcd"]

**Explanation:** We have the following: 
- For the string "abc", there is no substring that does not occur in any other string. 
- For the string "bcd", there is no substring that does not occur in any other string. 
- For the string "abcd", the shortest substring that does not occur in any other string is "abcd".

**Constraints:**

*   `n == arr.length`
*   `2 <= n <= 100`
*   `1 <= arr[i].length <= 20`
*   `arr[i]` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    private final Trie root = new Trie();

    public String[] shortestSubstrings(String[] arr) {
        int n = arr.length;
        for (int k = 0; k < n; ++k) {
            String s = arr[k];
            char[] cs = s.toCharArray();
            int m = cs.length;
            for (int i = 0; i < m; ++i) {
                insert(cs, i, m, k);
            }
        }
        String[] ans = new String[n];
        for (int k = 0; k < n; ++k) {
            String s = arr[k];
            char[] cs = s.toCharArray();
            int m = cs.length;
            String result = "";
            int resultLen = m + 1;
            for (int i = 0; i < m; ++i) {
                int curLen = search(cs, i, Math.min(m, i + resultLen), k);
                if (curLen != -1) {
                    String sub = new String(cs, i, curLen);
                    if (curLen < resultLen || result.compareTo(sub) > 0) {
                        result = sub;
                        resultLen = curLen;
                    }
                }
            }
            ans[k] = result;
        }
        return ans;
    }

    private void insert(char[] cs, int start, int end, int wordIndex) {
        Trie curr = root;
        for (int i = start; i < end; ++i) {
            int index = cs[i] - 'a';
            if (curr.children[index] == null) {
                curr.children[index] = new Trie();
            }
            curr = curr.children[index];
            if (curr.wordIndex == -1 || curr.wordIndex == wordIndex) {
                curr.wordIndex = wordIndex;
            } else {
                curr.wordIndex = -2;
            }
        }
    }

    private int search(char[] cs, int start, int end, int wordIndex) {
        Trie cur = root;
        for (int i = start; i < end; i++) {
            int index = cs[i] - 'a';
            cur = cur.children[index];
            if (cur.wordIndex == wordIndex) {
                return i - start + 1;
            }
        }
        return -1;
    }

    private static class Trie {
        Trie[] children;
        int wordIndex;

        public Trie() {
            children = new Trie[26];
            wordIndex = -1;
        }
    }
}
```