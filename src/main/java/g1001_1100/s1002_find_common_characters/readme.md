[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1002\. Find Common Characters

Easy

Given a string array `words`, return _an array of all characters that show up in all strings within the_ `words` _(including duplicates)_. You may return the answer in **any order**.

**Example 1:**

**Input:** words = ["bella","label","roller"]

**Output:** ["e","l","l"]

**Example 2:**

**Input:** words = ["cool","lock","cook"]

**Output:** ["c","o"]

**Constraints:**

*   `1 <= words.length <= 100`
*   `1 <= words[i].length <= 100`
*   `words[i]` consists of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("java:S112")
public class Solution {
    public List<String> commonChars(String[] words) {
        if (words == null) {
            throw new RuntimeException("words null");
        }
        if (words.length == 0) {
            return new ArrayList<>();
        }
        String tmp = words[0];
        for (int i = 1; i < words.length; i++) {
            tmp = getCommon(tmp, words[i]);
        }
        List<String> result = new ArrayList<>();
        for (int i = 0; i < tmp.length(); i++) {
            result.add(String.valueOf(tmp.charAt(i)));
        }
        return result;
    }

    private String getCommon(String s1, String s2) {
        if (s1.isEmpty() || s2.isEmpty()) {
            return "";
        }
        int[] c1c = countChars(s1);
        int[] c2c = countChars(s2);
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < c1c.length; i++) {
            int m = Math.min(c1c[i], c2c[i]);
            while (m > 0) {
                sb.append((char) ('a' + i));
                m--;
            }
        }
        return sb.toString();
    }

    private int[] countChars(String str) {
        int[] result = new int[26];
        for (int i = 0; i < str.length(); i++) {
            result[str.charAt(i) - 'a']++;
        }
        return result;
    }
}
```