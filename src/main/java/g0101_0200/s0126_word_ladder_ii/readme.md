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
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

@SuppressWarnings("java:S135")
public class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        Map<String, Set<String>> graph = new HashMap<>();
        int targetLength = bfs(graph, beginWord, endWord, wordList);
        List<List<String>> ans = new ArrayList<>();
        if (targetLength == 0) {
            return ans;
        }
        dfs(ans, new ArrayList<>(), targetLength, beginWord, endWord, graph);
        return ans;
    }

    private int bfs(
            Map<String, Set<String>> graph,
            String beginWord,
            String endWord,
            List<String> wordList) {
        Set<String> wordSet = new HashSet<>(wordList);
        if (!wordSet.contains(endWord)) {
            return 0;
        }
        wordSet.add(beginWord);
        // Bi BFS
        // two queue (startWord, endWord)
        Set<String> set1 = new HashSet<>(Arrays.asList(beginWord));
        Set<String> set2 = new HashSet<>(Arrays.asList(endWord));
        Set<String> visited = new HashSet<>();
        boolean isReversed = false;
        // the number of words in the transformation sequence
        int length = 0;
        boolean found = false;
        while (!set1.isEmpty() && !found) {
            Set<String> newSet = new HashSet<>();
            length++;
            // check next layer
            for (String word : set1) {
                visited.add(word);
                // check neighbor
                char[] arrWord = word.toCharArray();
                for (int i = 0; i < arrWord.length; i++) {
                    char oldChar = arrWord[i];
                    for (int j = 0; j < 26; j++) {
                        char newChar = (char) ('a' + j);
                        // skip original word
                        if (newChar == oldChar) {
                            continue;
                        }
                        arrWord[i] = newChar;
                        String newWord = String.valueOf(arrWord);
                        if (!wordSet.contains(newWord)) {
                            continue;
                        }
                        if (set2.contains(newWord)) {
                            found = true;
                        }
                        if (visited.contains(newWord)) {
                            continue;
                        }
                        newSet.add(newWord);
                        if (!isReversed) {
                            graph.putIfAbsent(word, new HashSet<>());
                            graph.get(word).add(newWord);
                        } else {
                            graph.putIfAbsent(newWord, new HashSet<>());
                            graph.get(newWord).add(word);
                        }
                    }
                    // revert change
                    arrWord[i] = oldChar;
                }
            }
            set1 = newSet;
            // swap queue
            // always make sure set1 is the smaller set
            // so we can end early if there is no path from begin to end word (set1 will be empty)
            if (set1.size() > set2.size()) {
                Set<String> tmpVisited = set1;
                set1 = set2;
                set2 = tmpVisited;
                isReversed = !isReversed;
            }
        }
        if (!found) {
            return 0;
        }
        return length + 1;
    }

    private void dfs(
            List<List<String>> ans,
            List<String> path,
            int targetLength,
            String beginWord,
            String endWord,
            Map<String, Set<String>> graph) {
        path.add(beginWord);
        if (path.size() > targetLength) {
            path.remove(path.size() - 1);
            return;
        }
        if (beginWord.equals(endWord)) {
            ans.add(new ArrayList<>(path));
            path.remove(path.size() - 1);
            return;
        }
        if (graph.containsKey(beginWord)) {
            for (String neighbor : graph.get(beginWord)) {
                dfs(ans, path, targetLength, neighbor, endWord, graph);
            }
        }
        path.remove(path.size() - 1);
    }
}
```