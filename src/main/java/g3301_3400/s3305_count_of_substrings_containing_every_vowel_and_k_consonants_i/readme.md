[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3305\. Count of Substrings Containing Every Vowel and K Consonants I

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

*   `5 <= word.length <= 250`
*   `word` consists only of lowercase English letters.
*   `0 <= k <= word.length - 5`

## Solution

```java
public class Solution {
    public int countOfSubstrings(String word, int k) {
        char[] arr = word.toCharArray();
        int[] map = new int[26];
        map[0]++;
        map['e' - 'a']++;
        map['i' - 'a']++;
        map['o' - 'a']++;
        map['u' - 'a']++;
        int need = 5;
        int ans = 0;
        int consCnt = 0;
        int j = 0;
        for (char c : arr) {
            while (j < arr.length && (need > 0 || consCnt < k)) {
                if (isVowel(arr[j])) {
                    map[arr[j] - 'a']--;
                    if (map[arr[j] - 'a'] == 0) {
                        need--;
                    }
                } else {
                    consCnt++;
                }
                j++;
            }
            if (need == 0 && consCnt == k) {
                ans++;
                int m = j;
                while (m < arr.length) {
                    if (isVowel(arr[m])) {
                        ans++;
                    } else {
                        break;
                    }
                    m++;
                }
            }
            if (isVowel(c)) {
                map[c - 'a']++;
                if (map[c - 'a'] == 1) {
                    need++;
                }
            } else {
                consCnt--;
            }
        }
        return ans;
    }

    private boolean isVowel(char ch) {
        return ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u';
    }
}
```