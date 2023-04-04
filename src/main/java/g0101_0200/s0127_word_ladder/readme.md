[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 127\. Word Ladder

Hard

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words <code>beginWord -> s<sub>1</sub> -> s<sub>2</sub> -> ... -> s<sub>k</sub></code> such that:

*   Every adjacent pair of words differs by a single letter.
*   Every <code>s<sub>i</sub></code> for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
*   <code>s<sub>k</sub> == endWord</code>

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _the **number of words** in the **shortest transformation sequence** from_ `beginWord` _to_ `endWord`_, or_ `0` _if no such sequence exists._

**Example 1:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]

**Output:** 5

**Explanation:** One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long. 

**Example 2:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]

**Output:** 0

**Explanation:** The endWord "cog" is not in wordList, therefore there is no valid transformation sequence. 

**Constraints:**

*   `1 <= beginWord.length <= 10`
*   `endWord.length == beginWord.length`
*   `1 <= wordList.length <= 5000`
*   `wordList[i].length == beginWord.length`
*   `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
*   `beginWord != endWord`
*   All the words in `wordList` are **unique**.

## Solution

```java
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordDict) {
        Set<String> beginSet = new HashSet<>();
        Set<String> endSet = new HashSet<>();
        Set<String> wordSet = new HashSet<>(wordDict);
        Set<String> visited = new HashSet<>();
        if (!wordDict.contains(endWord)) {
            return 0;
        }
        int len = 1;
        int strLen = beginWord.length();
        beginSet.add(beginWord);
        endSet.add(endWord);
        while (!beginSet.isEmpty() && !endSet.isEmpty()) {
            if (beginSet.size() > endSet.size()) {
                Set<String> temp = beginSet;
                beginSet = endSet;
                endSet = temp;
            }
            Set<String> tempSet = new HashSet<>();
            for (String s : beginSet) {
                char[] chars = s.toCharArray();
                for (int i = 0; i < strLen; i++) {
                    char old = chars[i];
                    for (char j = 'a'; j <= 'z'; j++) {
                        chars[i] = j;
                        String temp = new String(chars);
                        if (endSet.contains(temp)) {
                            return len + 1;
                        }
                        if (!visited.contains(temp) && wordSet.contains(temp)) {
                            tempSet.add(temp);
                            visited.add(temp);
                        }
                    }
                    chars[i] = old;
                }
            }
            beginSet = tempSet;
            len++;
        }
        return 0;
    }
}
```