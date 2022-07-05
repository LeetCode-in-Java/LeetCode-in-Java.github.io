[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2325\. Decode the Message

Easy

You are given the strings `key` and `message`, which represent a cipher key and a secret message, respectively. The steps to decode `message` are as follows:

1.  Use the **first** appearance of all 26 lowercase English letters in `key` as the **order** of the substitution table.
2.  Align the substitution table with the regular English alphabet.
3.  Each letter in `message` is then **substituted** using the table.
4.  Spaces `' '` are transformed to themselves.

*   For example, given <code>key = "<ins>**hap**</ins>p<ins>**y**</ins> <ins>**bo**</ins>y"</code> (actual key would have **at least one** instance of each letter in the alphabet), we have the partial substitution table of (`'h' -> 'a'`, `'a' -> 'b'`, `'p' -> 'c'`, `'y' -> 'd'`, `'b' -> 'e'`, `'o' -> 'f'`).

Return _the decoded message_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/05/08/ex1new4.jpg)

**Input:** key = "the quick brown fox jumps over the lazy dog", message = "vkbs bs t suepuv"

**Output:** "this is a secret"

**Explanation:** The diagram above shows the substitution table.

It is obtained by taking the first appearance of each letter in "<ins>**the**</ins> <ins>**quick**</ins> <ins>**brown**</ins> <ins>**f**</ins>o<ins>**x**</ins> <ins>**j**</ins>u<ins>**mps**</ins> o<ins>**v**</ins>er the <ins>**lazy**</ins> <ins>**d**</ins>o<ins>**g**</ins>".

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/05/08/ex2new.jpg)

**Input:** key = "eljuxhpwnyrdgtqkviszcfmabo", message = "zwx hnfx lqantp mnoeius ycgk vcnjrdb"

**Output:** "the five boxing wizards jump quickly"

**Explanation:** The diagram above shows the substitution table.

It is obtained by taking the first appearance of each letter in "<ins>**eljuxhpwnyrdgtqkviszcfmabo**</ins>".

**Constraints:**

*   `26 <= key.length <= 2000`
*   `key` consists of lowercase English letters and `' '`.
*   `key` contains every letter in the English alphabet (`'a'` to `'z'`) **at least once**.
*   `1 <= message.length <= 2000`
*   `message` consists of lowercase English letters and `' '`.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public String decodeMessage(String key, String message) {
        StringBuilder sb = new StringBuilder();
        Map<Character, Character> temp = new HashMap<>();
        char[] alphabet = new char[26];
        int itr = 0;
        for (char c = 'a'; c <= 'z'; ++c) {
            alphabet[c - 'a'] = c;
        }
        for (int i = 0; i < key.length(); i++) {
            if (!temp.containsKey(key.charAt(i)) && key.charAt(i) != ' ') {
                temp.put(key.charAt(i), alphabet[itr++]);
            }
        }
        for (int j = 0; j < message.length(); j++) {
            if (message.charAt(j) == ' ') {
                sb.append(' ');
            } else {
                char result = temp.get(message.charAt(j));
                sb.append(result);
            }
        }
        return sb.toString();
    }
}
```