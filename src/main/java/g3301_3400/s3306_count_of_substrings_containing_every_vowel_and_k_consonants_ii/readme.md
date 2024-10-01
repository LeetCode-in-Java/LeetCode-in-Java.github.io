[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3306\. Count of Substrings Containing Every Vowel and K Consonants II

Medium

You are given a string `word` and a **non-negative** integer `k`.

Return the total number of substrings of `word` that contain every vowel (`'a'`, `'e'`, `'i'`, `'o'`, and `'u'`) **at least** once and **exactly** `k` consonants.

**Example 1:**

**Input:** word = "aeioqq", k = 1

**Output:** 0

**Explanation:**

There is no substring with every vowel.

**Example 2:**

**Input:** word = "aeiou", k = 0

**Output:** 1

**Explanation:**

The only substring with every vowel and zero consonants is `word[0..4]`, which is `"aeiou"`.

**Example 3:**

**Input:** word = "ieaouqqieaouqq", k = 1

**Output:** 3

**Explanation:**

The substrings with every vowel and one consonant are:

*   `word[0..5]`, which is `"ieaouq"`.
*   `word[6..11]`, which is `"qieaou"`.
*   `word[7..12]`, which is `"ieaouq"`.

**Constraints:**

*   <code>5 <= word.length <= 2 * 10<sup>5</sup></code>
*   `word` consists only of lowercase English letters.
*   `0 <= k <= word.length - 5`

## Solution

```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

public class Solution {
    public long countOfSubstrings(String word, int k) {
        return countOfSubstringHavingAtleastXConsonants(word, k)
                - countOfSubstringHavingAtleastXConsonants(word, k + 1);
    }

    private long countOfSubstringHavingAtleastXConsonants(String word, int k) {
        int start = 0;
        int end = 0;
        Set<Character> vowels = new HashSet<>();
        vowels.add('a');
        vowels.add('e');
        vowels.add('i');
        vowels.add('o');
        vowels.add('u');
        int consonants = 0;
        Map<Character, Integer> map = new HashMap<>();
        long res = 0;
        while (end < word.length()) {
            char ch = word.charAt(end);
            // adding vowel or consonants;
            if (vowels.contains(ch)) {
                if (map.containsKey(ch)) {
                    map.put(ch, map.get(ch) + 1);
                } else {
                    map.put(ch, 1);
                }
            } else {
                consonants++;
            }
            // checking any valid string ispresent or not
            while (map.size() == 5 && consonants >= k) {
                res += word.length() - end;
                char ch1 = word.charAt(start);
                if (vowels.contains(ch1)) {
                    int temp = map.get(ch1) - 1;
                    if (temp == 0) {
                        map.remove(ch1);
                    } else {
                        map.put(ch1, temp);
                    }
                } else {
                    consonants--;
                }
                start++;
            }
            end++;
        }
        return res;
    }
}
```