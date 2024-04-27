[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3121\. Count the Number of Special Characters II

Medium

You are given a string `word`. A letter `c` is called **special** if it appears **both** in lowercase and uppercase in `word`, and **every** lowercase occurrence of `c` appears before the **first** uppercase occurrence of `c`.

Return the number of **special** letters in `word`.

**Example 1:**

**Input:** word = "aaAbcBC"

**Output:** 3

**Explanation:**

The special characters are `'a'`, `'b'`, and `'c'`.

**Example 2:**

**Input:** word = "abc"

**Output:** 0

**Explanation:**

There are no special characters in `word`.

**Example 3:**

**Input:** word = "AbBCab"

**Output:** 0

**Explanation:**

There are no special characters in `word`.

**Constraints:**

*   <code>1 <= word.length <= 2 * 10<sup>5</sup></code>
*   `word` consists of only lowercase and uppercase English letters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int numberOfSpecialChars(String word) {
        int[] small = new int[26];
        Arrays.fill(small, -1);
        int[] capital = new int[26];
        Arrays.fill(capital, Integer.MAX_VALUE);
        int result = 0;
        for (int i = 0; i < word.length(); i++) {
            char a = word.charAt(i);
            if (a < 91) {
                capital[a - 65] = Math.min(capital[a - 65], i);
            } else {
                small[a - 97] = i;
            }
        }
        for (int i = 0; i < 26; i++) {
            if (-1 != small[i] && Integer.MAX_VALUE != capital[i] && capital[i] > small[i]) {
                result++;
            }
        }
        return result;
    }
}
```