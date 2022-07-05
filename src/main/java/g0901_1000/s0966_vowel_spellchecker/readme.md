[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 966\. Vowel Spellchecker

Medium

Given a `wordlist`, we want to implement a spellchecker that converts a query word into a correct word.

For a given `query` word, the spell checker handles two categories of spelling mistakes:

*   Capitalization: If the query matches a word in the wordlist (**case-insensitive**), then the query word is returned with the same case as the case in the wordlist.
    *   Example: `wordlist = ["yellow"]`, `query = "YellOw"`: `correct = "yellow"`
    *   Example: `wordlist = ["Yellow"]`, `query = "yellow"`: `correct = "Yellow"`
    *   Example: `wordlist = ["yellow"]`, `query = "yellow"`: `correct = "yellow"`
*   Vowel Errors: If after replacing the vowels `('a', 'e', 'i', 'o', 'u')` of the query word with any vowel individually, it matches a word in the wordlist (**case-insensitive**), then the query word is returned with the same case as the match in the wordlist.
    *   Example: `wordlist = ["YellOw"]`, `query = "yollow"`: `correct = "YellOw"`
    *   Example: `wordlist = ["YellOw"]`, `query = "yeellow"`: `correct = ""` (no match)
    *   Example: `wordlist = ["YellOw"]`, `query = "yllw"`: `correct = ""` (no match)

In addition, the spell checker operates under the following precedence rules:

*   When the query exactly matches a word in the wordlist (**case-sensitive**), you should return the same word back.
*   When the query matches a word up to capitlization, you should return the first such match in the wordlist.
*   When the query matches a word up to vowel errors, you should return the first such match in the wordlist.
*   If the query has no matches in the wordlist, you should return the empty string.

Given some `queries`, return a list of words `answer`, where `answer[i]` is the correct word for `query = queries[i]`.

**Example 1:**

**Input:** wordlist = ["KiTe","kite","hare","Hare"], queries = ["kite","Kite","KiTe","Hare","HARE","Hear","hear","keti","keet","keto"]

**Output:** ["kite","KiTe","KiTe","Hare","hare","","","KiTe","","KiTe"]

**Example 2:**

**Input:** wordlist = ["yellow"], queries = ["YellOw"]

**Output:** ["yellow"]

**Constraints:**

*   `1 <= wordlist.length, queries.length <= 5000`
*   `1 <= wordlist[i].length, queries[i].length <= 7`
*   `wordlist[i]` and `queries[i]` consist only of only English letters.

## Solution

```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

public class Solution {
    private Set<String> matched;
    private Map<String, String> capitalizations;
    private Map<String, String> vowelErrors;

    private boolean isVowel(char w) {
        return w == 'a' || w == 'e' || w == 'i' || w == 'o' || w == 'u';
    }

    private String removeVowels(String word) {
        StringBuilder s = new StringBuilder();
        for (char w : word.toCharArray()) {
            s.append(isVowel(w) ? '*' : w);
        }
        return s.toString();
    }

    private String solveQuery(String query) {
        if (matched.contains(query)) {
            return query;
        }
        String word = query.toLowerCase();
        if (capitalizations.containsKey(word)) {
            return capitalizations.get(word);
        }
        word = removeVowels(word);
        if (vowelErrors.containsKey(word)) {
            return vowelErrors.get(word);
        }
        return "";
    }

    public String[] spellchecker(String[] wordlist, String[] queries) {
        String[] answer = new String[queries.length];
        matched = new HashSet<>();
        capitalizations = new HashMap<>();
        vowelErrors = new HashMap<>();
        for (String word : wordlist) {
            matched.add(word);
            String s = word.toLowerCase();
            capitalizations.putIfAbsent(s, word);
            s = removeVowels(s);
            vowelErrors.putIfAbsent(s, word);
        }
        for (int i = 0; i < queries.length; i++) {
            answer[i] = solveQuery(queries[i]);
        }
        return answer;
    }
}
```