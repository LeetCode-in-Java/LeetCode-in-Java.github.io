[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 30\. Substring with Concatenation of All Words

Hard

You are given a string `s` and an array of strings `words`. All the strings of `words` are of **the same length**.

A **concatenated string** is a string that exactly contains all the strings of any permutation of `words` concatenated.

*   For example, if `words = ["ab","cd","ef"]`, then `"abcdef"`, `"abefcd"`, `"cdabef"`, `"cdefab"`, `"efabcd"`, and `"efcdab"` are all concatenated strings. `"acdbef"` is not a concatenated string because it is not the concatenation of any permutation of `words`.

Return an array of _the starting indices_ of all the concatenated substrings in `s`. You can return the answer in **any order**.

**Example 1:**

**Input:** s = "barfoothefoobarman", words = ["foo","bar"]

**Output:** [0,9]

**Explanation:**

The substring starting at 0 is `"barfoo"`. It is the concatenation of `["bar","foo"]` which is a permutation of `words`.   
 The substring starting at 9 is `"foobar"`. It is the concatenation of `["foo","bar"]` which is a permutation of `words`.

**Example 2:**

**Input:** s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]

**Output:** []

**Explanation:**

There is no concatenated substring.

**Example 3:**

**Input:** s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]

**Output:** [6,9,12]

**Explanation:**

The substring starting at 6 is `"foobarthe"`. It is the concatenation of `["foo","bar","the"]`.   
 The substring starting at 9 is `"barthefoo"`. It is the concatenation of `["bar","the","foo"]`.   
 The substring starting at 12 is `"thefoobar"`. It is the concatenation of `["the","foo","bar"]`.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `1 <= words.length <= 5000`
*   `1 <= words[i].length <= 30`
*   `s` and `words[i]` consist of lowercase English letters.

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