## 316\. Remove Duplicate Letters

Medium

Given a string `s`, remove duplicate letters so that every letter appears once and only once. You must make sure your result is **the smallest in lexicographical order** among all possible results.

**Example 1:**

**Input:** s = "bcabc"

**Output:** "abc" 

**Example 2:**

**Input:** s = "cbacdcbc"

**Output:** "acdb" 

**Constraints:**

*   <code>1 <= s.length <= 10<sup>4</sup></code>
*   `s` consists of lowercase English letters.

**Note:** This question is the same as 1081.

## Solution

```java
public class Solution {
    public String removeDuplicateLetters(String s) {
        int[] charCount = new int[26];
        boolean[] charAdded = new boolean[26];
        // Build the char count array
        for (char c : s.toCharArray()) {
            charCount[c - 'a'] += 1;
        }
        StringBuilder sb = new StringBuilder();
        // i = index of the input string
        // j = index of the output stringBuilder
        int j = 0;
        for (int i = 0; i < s.length(); i++) {
            char curr = s.charAt(i);
            // If the curr char is NOT already added in the final string
            if (!charAdded[curr - 'a']) {
                // If the prev char in final string is lexicographically greater than curr char of
                // input string
                // And there are more characters in charCount array then we can remove this prev
                // char from final string
                // Do this check iteratively until all characters are removed from the final string
                // or prev char < curr char
                while (j > 0 && sb.charAt(j - 1) > curr && charCount[sb.charAt(j - 1) - 'a'] > 0) {
                    charAdded[sb.charAt(j - 1) - 'a'] = false;
                    sb.deleteCharAt(j - 1);
                    j--;
                }
                // Add the curr char in final string and mark that character as added in the string
                sb.append(curr);
                charAdded[curr - 'a'] = true;
                j++;
            }
            // Reduce the count of the current character from the charCount array
            charCount[curr - 'a'] -= 1;
        }
        return sb.toString();
    }
}
```