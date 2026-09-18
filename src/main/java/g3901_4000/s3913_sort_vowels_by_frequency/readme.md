[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3913\. Sort Vowels by Frequency

Medium

You are given a string `s` consisting of lowercase English characters.

Rearrange only the **vowels** in the string so that they appear in **non-increasing** order of their frequency.

If multiple vowels have the same **frequency**, order them by the position of their **first occurrence** in `s`.

Return the modified string.

Vowels are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

The **frequency** of a letter is the number of times it occurs in the string.

**Example 1:**

**Input:** s = "leetcode"

**Output:** "leetcedo"

**Explanation:**

*   Vowels in the string are `['e', 'e', 'o', 'e']` with frequencies: `e = 3`, `o = 1`.
*   Sorting in non-increasing order of frequency and placing them back into the vowel positions results in `"leetcedo"`.

**Example 2:**

**Input:** s = "aeiaaioooa"

**Output:** "aaaaoooiie"

**Explanation:**

*   Vowels in the string are `['a', 'e', 'i', 'a', 'a', 'i', 'o', 'o', 'o', 'a']` with frequencies: `a = 4`, `o = 3`, `i = 2`, `e = 1`.
*   Sorting them in non-increasing order of frequency and placing them back into the vowel positions results in `"aaaaoooiie"`.

**Example 3:**

**Input:** s = "baeiou"

**Output:** "baeiou"

**Explanation:**

*   Each vowel appears exactly once, so all have the same frequency.
*   Thus, they retain their relative order based on first occurrence, and the string remains unchanged.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public String sortVowels(String s) {
        int[] freq = new int[26];
        char[] ch = s.toCharArray();
        for (char c : ch) {
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                freq[c - 'a']++;
            }
        }
        List<int[]> x = new ArrayList<>();
        for (int i = 0; i < 26; i++) {
            if (freq[i] > 0) {
                x.add(new int[] {i, freq[i]});
            }
        }
        x.sort(
                (a, b) ->
                        b[1] - a[1] == 0
                                ? s.indexOf((char) (a[0] + 'a')) - s.indexOf((char) (b[0] + 'a'))
                                : b[1] - a[1]);
        int i = 0;
        for (int[] f : x) {
            while (f[1] > 0) {
                char c = ch[i];
                if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                    ch[i] = (char) (f[0] + 'a');
                    f[1]--;
                }
                i++;
            }
        }
        return new String(ch);
    }
}
```