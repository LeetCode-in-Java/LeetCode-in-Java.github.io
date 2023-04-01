[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 824\. Goat Latin

Easy

You are given a string `sentence` that consist of words separated by spaces. Each word consists of lowercase and uppercase letters only.

We would like to convert the sentence to "Goat Latin" (a made-up language similar to Pig Latin.) The rules of Goat Latin are as follows:

*   If a word begins with a vowel (`'a'`, `'e'`, `'i'`, `'o'`, or `'u'`), append `"ma"` to the end of the word.
    *   For example, the word `"apple"` becomes `"applema"`.
*   If a word begins with a consonant (i.e., not a vowel), remove the first letter and append it to the end, then add `"ma"`.
    *   For example, the word `"goat"` becomes `"oatgma"`.
*   Add one letter `'a'` to the end of each word per its word index in the sentence, starting with `1`.
    *   For example, the first word gets `"a"` added to the end, the second word gets `"aa"` added to the end, and so on.

Return _the final sentence representing the conversion from sentence to Goat Latin_.

**Example 1:**

**Input:** sentence = "I speak Goat Latin"

**Output:** "Imaa peaksmaaa oatGmaaaa atinLmaaaaa"

**Example 2:**

**Input:** sentence = "The quick brown fox jumped over the lazy dog"

**Output:** "heTmaa uickqmaaa rownbmaaaa oxfmaaaaa umpedjmaaaaaa overmaaaaaaa hetmaaaaaaaa azylmaaaaaaaaa ogdmaaaaaaaaaa"

**Constraints:**

*   `1 <= sentence.length <= 150`
*   `sentence` consists of English letters and spaces.
*   `sentence` has no leading or trailing spaces.
*   All the words in `sentence` are separated by a single space.

## Solution

```java
public class Solution {
    public String toGoatLatin(String sentence) {
        String[] splits = sentence.split(" ");
        StringBuilder sb = new StringBuilder();
        StringBuilder a = new StringBuilder();
        for (String word : splits) {
            if (isVowel(word.charAt(0))) {
                sb.append(word).append("ma");
            } else {
                char firstChar = word.charAt(0);
                sb.append(word.substring(1)).append(firstChar).append("ma");
            }
            a.append("a");
            sb.append(a);
            sb.append(" ");
        }
        return sb.toString().trim();
    }

    private boolean isVowel(char c) {
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' || c == 'A' || c == 'E'
                || c == 'I' || c == 'O' || c == 'U';
    }
}
```