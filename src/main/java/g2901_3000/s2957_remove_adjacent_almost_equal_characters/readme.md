[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2957\. Remove Adjacent Almost-Equal Characters

Medium

You are given a **0-indexed** string `word`.

In one operation, you can pick any index `i` of `word` and change `word[i]` to any lowercase English letter.

Return _the **minimum** number of operations needed to remove all adjacent **almost-equal** characters from_ `word`.

Two characters `a` and `b` are **almost-equal** if `a == b` or `a` and `b` are adjacent in the alphabet.

**Example 1:**

**Input:** word = "aaaaa"

**Output:** 2

**Explanation:** We can change word into "a**<ins>c</ins>**a<ins>**c**</ins>a" which does not have any adjacent almost-equal characters. 

It can be shown that the minimum number of operations needed to remove all adjacent almost-equal characters from word is 2.

**Example 2:**

**Input:** word = "abddez"

**Output:** 2

**Explanation:** We can change word into "**<ins>y</ins>**bd<ins>**o**</ins>ez" which does not have any adjacent almost-equal characters. 

It can be shown that the minimum number of operations needed to remove all adjacent almost-equal characters from word is 2.

**Example 3:**

**Input:** word = "zyxyxyz"

**Output:** 3

**Explanation:** We can change word into "z<ins>**a**</ins>x<ins>**a**</ins>x**<ins>a</ins>**z" which does not have any adjacent almost-equal characters.

It can be shown that the minimum number of operations needed to remove all adjacent almost-equal characters from word is 3.

**Constraints:**

*   `1 <= word.length <= 100`
*   `word` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public int removeAlmostEqualCharacters(String word) {
        int count = 0;
        char[] wordArray = word.toCharArray();
        for (int i = 1; i < wordArray.length; i++) {
            if (Math.abs(wordArray[i] - wordArray[i - 1]) <= 1) {
                count++;
                wordArray[i] =
                        (i + 1 < wordArray.length
                                        && (wordArray[i + 1] != 'a' && wordArray[i + 1] != 'b'))
                                ? 'a'
                                : 'z';
            }
        }
        return count;
    }
}
```