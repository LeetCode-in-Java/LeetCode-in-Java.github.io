[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2062\. Count Vowel Substrings of a String

Easy

A **substring** is a contiguous (non-empty) sequence of characters within a string.

A **vowel substring** is a substring that **only** consists of vowels (`'a'`, `'e'`, `'i'`, `'o'`, and `'u'`) and has **all five** vowels present in it.

Given a string `word`, return _the number of **vowel substrings** in_ `word`.

**Example 1:**

**Input:** word = "aeiouu"

**Output:** 2

**Explanation:** The vowel substrings of word are as follows (underlined): 

- "**aeiou**u" 

- "**aeiouu**"

**Example 2:**

**Input:** word = "unicornarihan"

**Output:** 0

**Explanation:** Not all 5 vowels are present, so there are no vowel substrings.

**Example 3:**

**Input:** word = "cuaieuouac"

**Output:** 7

**Explanation:** The vowel substrings of word are as follows (underlined): 

- "c**uaieuo**uac" 

- "c**uaieuou**ac" 

- "c**uaieuoua**c" 

- "cu**aieuo**uac" 

- "cu**aieuou**ac" 

- "cu**aieuoua**c" 

- "cua**ieuoua**c"

**Constraints:**

*   `1 <= word.length <= 100`
*   `word` consists of lowercase English letters only.

## Solution

```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int countVowelSubstrings(String word) {
        int count = 0;
        Set<Character> vowels = new HashSet<>(Arrays.asList('a', 'e', 'i', 'o', 'u'));
        Set<Character> window = new HashSet<>();
        for (int i = 0; i < word.length(); i++) {
            window.clear();
            if (vowels.contains(word.charAt(i))) {
                window.add(word.charAt(i));
                for (int j = i + 1; j < word.length(); j++) {
                    if (!vowels.contains(word.charAt(j))) {
                        break;
                    } else {
                        window.add(word.charAt(j));
                        if (window.size() == 5) {
                            count++;
                        }
                    }
                }
            }
        }
        return count;
    }
}
```