[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 126\. Word Ladder II

Hard

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words <code>beginWord -> s<sub>1</sub> -> s<sub>2</sub> -> ... -> s<sub>k</sub></code> such that:

*   Every adjacent pair of words differs by a single letter.
*   Every <code>s<sub>i</sub></code> for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
*   <code>s<sub>k</sub> == endWord</code>

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _all the **shortest transformation sequences** from_ `beginWord` _to_ `endWord`_, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words_ <code>[beginWord, s<sub>1</sub>, s<sub>2</sub>, ..., s<sub>k</sub>]</code>.

**Example 1:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]

**Output:** [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]

**Explanation:** There are 2 shortest transformation sequences: "hit" -> "hot" -> "dot" -> "dog" -> "cog" "hit" -> "hot" -> "lot" -> "log" -> "cog" 

**Example 2:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]

**Output:** []

**Explanation:** The endWord "cog" is not in wordList, therefore there is no valid transformation sequence. 

**Constraints:**

*   `1 <= beginWord.length <= 5`
*   `endWord.length == beginWord.length`
*   `1 <= wordList.length <= 1000`
*   `wordList[i].length == beginWord.length`
*   `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
*   `beginWord != endWord`
*   All the words in `wordList` are **unique**.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;
import java.util.Queue;
import java.util.Set;

public class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> ans = new ArrayList<>();
        // reverse graph start from endWord
        Map<String, Set<String>> reverse = new HashMap<>();
        // remove the duplicate words
        Set<String> wordSet = new HashSet<>(wordList);
        // remove the first word to avoid cycle path
        wordSet.remove(beginWord);
        // store current layer nodes
        Queue<String> queue = new LinkedList<>();
        // first layer has only beginWord
        queue.add(beginWord);
        // store nextLayer nodes
        Set<String> nextLevel = new HashSet<>();
        // find endWord flag
        boolean findEnd = false;
        // traverse current layer nodes
        while (!queue.isEmpty()) {
            String word = queue.remove();
            for (String next : wordSet) {
                // is ladder words
                if (isLadder(word, next)) {
                    // construct the reverse graph from endWord
                    Set<String> reverseLadders =
                            reverse.computeIfAbsent(next, k -> new HashSet<>());
                    reverseLadders.add(word);
                    if (endWord.equals(next)) {
                        findEnd = true;
                    }
                    // store next layer nodes
                    nextLevel.add(next);
                }
            }
            // when current layer is all visited
            if (queue.isEmpty()) {
                // if find the endWord, then break the while loop
                if (findEnd) {
                    break;
                }
                // add next layer nodes to queue
                queue.addAll(nextLevel);
                // remove all next layer nodes in wordSet
                wordSet.removeAll(nextLevel);
                nextLevel.clear();
            }
        }
        // if can't reach endWord from startWord, then return ans.
        if (!findEnd) {
            return ans;
        }
        Set<String> path = new LinkedHashSet<>();
        path.add(endWord);
        // traverse reverse graph from endWord to beginWord
        findPath(endWord, beginWord, reverse, ans, path);
        return ans;
    }

    private void findPath(
            String endWord,
            String beginWord,
            Map<String, Set<String>> graph,
            List<List<String>> ans,
            Set<String> path) {
        Set<String> next = graph.get(endWord);
        if (next == null) {
            return;
        }
        for (String word : next) {
            path.add(word);
            if (beginWord.equals(word)) {
                List<String> shortestPath = new ArrayList<>(path);
                // reverse words in shortest path
                Collections.reverse(shortestPath);
                // add the shortest path to ans.
                ans.add(shortestPath);
            } else {
                findPath(word, beginWord, graph, ans, path);
            }
            path.remove(word);
        }
    }

    private boolean isLadder(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int diffCount = 0;
        int n = s.length();
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) != t.charAt(i)) {
                diffCount++;
            }
            if (diffCount > 1) {
                return false;
            }
        }
        return diffCount == 1;
    }
}
```