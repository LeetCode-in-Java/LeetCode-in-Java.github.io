## 520\. Detect Capital

Easy

We define the usage of capitals in a word to be right when one of the following cases holds:

*   All letters in this word are capitals, like `"USA"`.
*   All letters in this word are not capitals, like `"leetcode"`.
*   Only the first letter in this word is capital, like `"Google"`.

Given a string `word`, return `true` if the usage of capitals in it is right.

**Example 1:**

**Input:** word = "USA"

**Output:** true

**Example 2:**

**Input:** word = "FlaG"

**Output:** false

**Constraints:**

*   `1 <= word.length <= 100`
*   `word` consists of lowercase and uppercase English letters.

## Solution

```java
public class Solution {
    public boolean detectCapitalUse(String word) {
        if (word == null || word.length() == 0) {
            return false;
        }
        int upper = 0;
        int lower = 0;
        int n = word.length();
        boolean firstUpper = Character.isUpperCase(word.charAt(0));
        for (int i = 0; i < n; i++) {
            if (Character.isUpperCase(word.charAt(i))) {
                upper++;
            } else if (Character.isLowerCase(word.charAt(i))) {
                lower++;
            }
        }
        if (firstUpper && upper > 1) {
            firstUpper = false;
        }
        return upper == n || lower == n || firstUpper;
    }
}
```