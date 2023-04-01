[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 819\. Most Common Word

Easy

Given a string `paragraph` and a string array of the banned words `banned`, return _the most frequent word that is not banned_. It is **guaranteed** there is **at least one word** that is not banned, and that the answer is **unique**.

The words in `paragraph` are **case-insensitive** and the answer should be returned in **lowercase**.

**Example 1:**

**Input:** paragraph = "Bob hit a ball, the hit BALL flew far after it was hit.", banned = ["hit"]

**Output:** "ball"

**Explanation:** "hit" occurs 3 times, but it is a banned word. "ball" occurs twice (and no other word does), so it is the most frequent non-banned word in the paragraph. Note that words in the paragraph are not case sensitive, that punctuation is ignored (even if adjacent to words, such as "ball,"), and that "hit" isn't the answer even though it occurs more because it is banned.

**Example 2:**

**Input:** paragraph = "a.", banned = []

**Output:** "a"

**Constraints:**

*   `1 <= paragraph.length <= 1000`
*   paragraph consists of English letters, space `' '`, or one of the symbols: `"!?',;."`.
*   `0 <= banned.length <= 100`
*   `1 <= banned[i].length <= 10`
*   `banned[i]` consists of only lowercase English letters.

## Solution

```java
import java.util.Collections;
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public String mostCommonWord(String paragraph, String[] banned) {
        paragraph = paragraph.replaceAll("\\p{Punct}", " ").toLowerCase();
        String[] a = paragraph.split(" ");
        for (int i = 0; i < banned.length; i++) {
            banned[i] = banned[i].toLowerCase();
        }
        Map<String, Integer> map = new HashMap<>();
        for (String s : a) {
            int x = map.getOrDefault(s, 0);
            map.put(s, x + 1);
        }
        for (String s : banned) {
            map.remove(s);
            map.remove("");
        }
        return Collections.max(map.entrySet(), Map.Entry.comparingByValue()).getKey();
    }
}
```