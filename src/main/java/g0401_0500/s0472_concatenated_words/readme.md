[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 472\. Concatenated Words

Hard

Given an array of strings `words` (**without duplicates**), return _all the **concatenated words** in the given list of_ `words`.

A **concatenated word** is defined as a string that is comprised entirely of at least two shorter words in the given array.

**Example 1:**

**Input:** words = ["cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat"]

**Output:** ["catsdogcats","dogcatsdog","ratcatdogcat"]

**Explanation:** "catsdogcats" can be concatenated by "cats", "dog" and "cats"; "dogcatsdog" can be concatenated by "dog", "cats" and "dog"; "ratcatdogcat" can be concatenated by "rat", "cat", "dog" and "cat".

**Example 2:**

**Input:** words = ["cat","dog","catdog"]

**Output:** ["catdog"]

**Constraints:**

*   <code>1 <= words.length <= 10<sup>4</sup></code>
*   `0 <= words[i].length <= 1000`
*   `words[i]` consists of only lowercase English letters.
*   <code>0 <= sum(words[i].length) <= 10<sup>5</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    private final List<String> ans = new ArrayList<>();
    private Trie root;

    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        root = new Trie();
        Arrays.sort(words, (a, b) -> Integer.compare(a.length(), b.length()));
        for (String word : words) {
            Trie ptr = root;
            if (search(word, 0, 0)) {
                ans.add(word);
            } else {
                for (int j = 0; j < word.length(); j++) {
                    if (ptr.nxt[word.charAt(j) - 'a'] == null) {
                        ptr.nxt[word.charAt(j) - 'a'] = new Trie();
                    }
                    ptr = ptr.nxt[word.charAt(j) - 'a'];
                }
                ptr.endHere = true;
            }
        }
        return ans;
    }

    private boolean search(String cur, int idx, int wordCnt) {
        if (idx == cur.length()) {
            return wordCnt >= 2;
        }
        Trie ptr = root;
        for (int i = idx; i < cur.length(); i++) {
            if (ptr.nxt[cur.charAt(i) - 'a'] == null) {
                return false;
            }
            if (ptr.nxt[cur.charAt(i) - 'a'].endHere) {
                boolean ret = search(cur, i + 1, wordCnt + 1);
                if (ret) {
                    return true;
                }
            }
            ptr = ptr.nxt[cur.charAt(i) - 'a'];
        }
        return ptr.endHere && wordCnt >= 2;
    }

    private static class Trie {
        Trie[] nxt;
        boolean endHere;

        Trie() {
            nxt = new Trie[26];
            endHere = false;
        }
    }
}
```