[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 140\. Word Break II

Hard

Given a string `s` and a dictionary of strings `wordDict`, add spaces in `s` to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in **any order**.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

**Input:** s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]

**Output:** ["cats and dog","cat sand dog"] 

**Example 2:**

**Input:** s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]

**Output:** ["pine apple pen apple","pineapple pen apple","pine applepen apple"]

**Explanation:** Note that you are allowed to reuse a dictionary word. 

**Example 3:**

**Input:** s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]

**Output:** [] 

**Constraints:**

*   `1 <= s.length <= 20`
*   `1 <= wordDict.length <= 1000`
*   `1 <= wordDict[i].length <= 10`
*   `s` and `wordDict[i]` consist of only lowercase English letters.
*   All the strings of `wordDict` are **unique**.

## Solution

```java
import java.util.HashSet;
import java.util.LinkedList;
import java.util.List;
import java.util.Set;

public class Solution {
    public List<String> wordBreak(String s, List<String> wordDict) {
        List<String> result = new LinkedList<>();
        Set<String> wordSet = new HashSet<>(wordDict);
        dfs(s, wordSet, 0, new StringBuilder(), result);
        return result;
    }

    private void dfs(
            String s, Set<String> wordSet, int index, StringBuilder sb, List<String> result) {
        if (index == s.length()) {
            if (sb.charAt(sb.length() - 1) == ' ') {
                sb.setLength(sb.length() - 1);
            }
            result.add(sb.toString());
            return;
        }
        int len = sb.length();
        for (int i = index + 1; i <= s.length(); ++i) {
            String subs = s.substring(index, i);
            if (wordSet.contains(subs)) {
                sb.append(subs).append(" ");
                dfs(s, wordSet, i, sb, result);
            }
            sb.setLength(len);
        }
    }
}
```