[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 30\. Substring with Concatenation of All Words

Hard

You are given a string `s` and an array of strings `words` of **the same length**. Return all starting indices of substring(s) in `s` that is a concatenation of each word in `words` **exactly once**, **in any order**, and **without any intervening characters**.

You can return the answer in **any order**.

**Example 1:**

**Input:** s = "barfoothefoobarman", words = ["foo","bar"]

**Output:** [0,9]

**Explanation:** Substrings starting at index 0 and 9 are "barfoo" and "foobar" respectively. The output order does not matter, returning [9,0] is fine too. 

**Example 2:**

**Input:** s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]

**Output:** [] 

**Example 3:**

**Input:** s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]

**Output:** [6,9,12] 

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of lower-case English letters.
*   `1 <= words.length <= 5000`
*   `1 <= words[i].length <= 30`
*   `words[i]` consists of lower-case English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@SuppressWarnings("java:S127")
public class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> indices = new ArrayList<>();
        if (words.length == 0) {
            return indices;
        }
        // Put each word into a HashMap and calculate word frequency
        Map<String, Integer> wordMap = new HashMap<>();
        for (String word : words) {
            wordMap.put(word, wordMap.getOrDefault(word, 0) + 1);
        }
        int wordLength = words[0].length();
        int window = words.length * wordLength;
        for (int i = 0; i < wordLength; i++) {
            // move a word's length each time
            for (int j = i; j + window <= s.length(); j = j + wordLength) {
                // get the subStr
                String subStr = s.substring(j, j + window);
                Map<String, Integer> map = new HashMap<>();
                // start from the last word
                for (int k = words.length - 1; k >= 0; k--) {
                    // get the word from subStr
                    String word = subStr.substring(k * wordLength, (k + 1) * wordLength);
                    int count = map.getOrDefault(word, 0) + 1;
                    // if the num of the word is greater than wordMap's, move (k * wordLength) and
                    // break
                    if (count > wordMap.getOrDefault(word, 0)) {
                        j = j + k * wordLength;
                        break;
                    } else if (k == 0) {
                        indices.add(j);
                    } else {
                        map.put(word, count);
                    }
                }
            }
        }
        return indices;
    }
}
```