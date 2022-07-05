[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2000\. Reverse Prefix of Word

Easy

Given a **0-indexed** string `word` and a character `ch`, **reverse** the segment of `word` that starts at index `0` and ends at the index of the **first occurrence** of `ch` (**inclusive**). If the character `ch` does not exist in `word`, do nothing.

*   For example, if `word = "abcdefd"` and `ch = "d"`, then you should **reverse** the segment that starts at `0` and ends at `3` (**inclusive**). The resulting string will be `"dcbaefd"`.

Return _the resulting string_.

**Example 1:**

**Input:** word = "abcdefd", ch = "d"

**Output:** "dcbaefd"

**Explanation:** The first occurrence of "d" is at index 3.

Reverse the part of word from 0 to 3 (inclusive), the resulting string is "dcbaefd". 

**Example 2:**

**Input:** word = "xyxzxe", ch = "z"

**Output:** "zxyxxe"

**Explanation:** The first and only occurrence of "z" is at index 3.

Reverse the part of word from 0 to 3 (inclusive), the resulting string is "zxyxxe". 

**Example 3:**

**Input:** word = "abcd", ch = "z"

**Output:** "abcd"

**Explanation:** "z" does not exist in word.

You should not do any reverse operation, the resulting string is "abcd". 

**Constraints:**

*   `1 <= word.length <= 250`
*   `word` consists of lowercase English letters.
*   `ch` is a lowercase English letter.

## Solution

```java
public class Solution {
    public String reversePrefix(String word, char ch) {
        int i = 0;
        int j = word.indexOf(ch);
        char[] charArr = word.toCharArray();
        while (i < j) {
            char temp = charArr[i];
            charArr[i] = charArr[j];
            charArr[j] = temp;
            i++;
            j--;
        }
        return String.valueOf(charArr);
    }
}
```