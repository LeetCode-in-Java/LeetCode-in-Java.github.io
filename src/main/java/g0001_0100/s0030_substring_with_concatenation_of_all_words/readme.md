[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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

public class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> ans = new ArrayList<>();
        int n1 = words[0].length();
        int n2 = s.length();
        Map<String, Integer> map1 = new HashMap<>();
        for (String ch : words) {
            map1.put(ch, map1.getOrDefault(ch, 0) + 1);
        }
        for (int i = 0; i < n1; i++) {
            int left = i;
            int j = i;
            int c = 0;
            Map<String, Integer> map2 = new HashMap<>();
            while (j + n1 <= n2) {
                String word1 = s.substring(j, j + n1);
                j += n1;
                if (map1.containsKey(word1)) {
                    map2.put(word1, map2.getOrDefault(word1, 0) + 1);
                    c++;
                    while (map2.get(word1) > map1.get(word1)) {
                        String word2 = s.substring(left, left + n1);
                        map2.put(word2, map2.get(word2) - 1);
                        left += n1;
                        c--;
                    }
                    if (c == words.length) {
                        ans.add(left);
                    }
                } else {
                    map2.clear();
                    c = 0;
                    left = j;
                }
            }
        }

        return ans;
    }
}
```