[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3304\. Find the K-th Character in String Game I

Easy

Alice and Bob are playing a game. Initially, Alice has a string `word = "a"`.

You are given a **positive** integer `k`.

Now Bob will ask Alice to perform the following operation **forever**:

*   Generate a new string by **changing** each character in `word` to its **next** character in the English alphabet, and **append** it to the _original_ `word`.

For example, performing the operation on `"c"` generates `"cd"` and performing the operation on `"zb"` generates `"zbac"`.

Return the value of the <code>k<sup>th</sup></code> character in `word`, after enough operations have been done for `word` to have **at least** `k` characters.

**Note** that the character `'z'` can be changed to `'a'` in the operation.

**Example 1:**

**Input:** k = 5

**Output:** "b"

**Explanation:**

Initially, `word = "a"`. We need to do the operation three times:

*   Generated string is `"b"`, `word` becomes `"ab"`.
*   Generated string is `"bc"`, `word` becomes `"abbc"`.
*   Generated string is `"bccd"`, `word` becomes `"abbcbccd"`.

**Example 2:**

**Input:** k = 10

**Output:** "c"

**Constraints:**

*   `1 <= k <= 500`

## Solution

```java
public class Solution {
    public char kthCharacter(int k) {
        // Initialize the length of the current string
        // Initial length when word = "a"
        int length = 1;

        // Find the total length after enough iterations
        while (length < k) {
            length *= 2;
        }
        // Trace back to the original character
        // Start with 'a'
        char currentChar = 'a';
        while (length > 1) {
            length /= 2;
            if (k > length) {
                // Adjust k for the next character
                k -= length;
                // Move to the next character
                currentChar++;
                if (currentChar > 'z') {
                    // Wrap around if exceeds 'z'
                    currentChar = 'a';
                }
            }
        }
        return currentChar;
    }
}
```