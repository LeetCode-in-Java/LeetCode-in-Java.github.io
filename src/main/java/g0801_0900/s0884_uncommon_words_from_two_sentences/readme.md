[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 884\. Uncommon Words from Two Sentences

Easy

A **sentence** is a string of single-space separated words where each word consists only of lowercase letters.

A word is **uncommon** if it appears exactly once in one of the sentences, and **does not appear** in the other sentence.

Given two **sentences** `s1` and `s2`, return _a list of all the **uncommon words**_. You may return the answer in **any order**.

**Example 1:**

**Input:** s1 = "this apple is sweet", s2 = "this apple is sour"

**Output:** ["sweet","sour"]

**Example 2:**

**Input:** s1 = "apple apple", s2 = "banana"

**Output:** ["banana"]

**Constraints:**

*   `1 <= s1.length, s2.length <= 200`
*   `s1` and `s2` consist of lowercase English letters and spaces.
*   `s1` and `s2` do not have leading or trailing spaces.
*   All the words in `s1` and `s2` are separated by a single space.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public String[] uncommonFromSentences(String s1, String s2) {
        Map<String, Integer> map = new HashMap<>();
        for (String word : s1.split(" ")) {
            map.put(word, map.getOrDefault(word, 0) + 1);
        }
        for (String word : s2.split(" ")) {
            map.put(word, map.getOrDefault(word, 0) + 1);
        }
        List<String> result = new ArrayList<>();
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            if (entry.getValue() == 1) {
                result.add(entry.getKey());
            }
        }
        String[] strs = new String[result.size()];
        result.toArray(strs);
        return strs;
    }
}
```