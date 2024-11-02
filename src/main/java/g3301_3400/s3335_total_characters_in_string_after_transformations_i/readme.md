[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3335\. Total Characters in String After Transformations I

Medium

You are given a string `s` and an integer `t`, representing the number of **transformations** to perform. In one **transformation**, every character in `s` is replaced according to the following rules:

*   If the character is `'z'`, replace it with the string `"ab"`.
*   Otherwise, replace it with the **next** character in the alphabet. For example, `'a'` is replaced with `'b'`, `'b'` is replaced with `'c'`, and so on.

Return the **length** of the resulting string after **exactly** `t` transformations.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** s = "abcyy", t = 2

**Output:** 7

**Explanation:**

*   **First Transformation (t = 1)**:
    *   `'a'` becomes `'b'`
    *   `'b'` becomes `'c'`
    *   `'c'` becomes `'d'`
    *   `'y'` becomes `'z'`
    *   `'y'` becomes `'z'`
    *   String after the first transformation: `"bcdzz"`
*   **Second Transformation (t = 2)**:
    *   `'b'` becomes `'c'`
    *   `'c'` becomes `'d'`
    *   `'d'` becomes `'e'`
    *   `'z'` becomes `"ab"`
    *   `'z'` becomes `"ab"`
    *   String after the second transformation: `"cdeabab"`
*   **Final Length of the string**: The string is `"cdeabab"`, which has 7 characters.

**Example 2:**

**Input:** s = "azbk", t = 1

**Output:** 5

**Explanation:**

*   **First Transformation (t = 1)**:
    *   `'a'` becomes `'b'`
    *   `'z'` becomes `"ab"`
    *   `'b'` becomes `'c'`
    *   `'k'` becomes `'l'`
    *   String after the first transformation: `"babcl"`
*   **Final Length of the string**: The string is `"babcl"`, which has 5 characters.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists only of lowercase English letters.
*   <code>1 <= t <= 10<sup>5</sup></code>

## Solution

```java
import java.util.LinkedList;

public class Solution {
    public int lengthAfterTransformations(String s, int t) {
        int[] count = new int[26];
        for (char c : s.toCharArray()) {
            count[c - 'a']++;
        }
        LinkedList<Integer> list = new LinkedList<>();
        for (int c : count) {
            list.add(c);
        }
        int delta = s.length() % 1000000007;
        for (int i = 0; i < t; i++) {
            int zCount = list.removeLast() % 1000000007;
            int aCount = list.pollFirst() % 1000000007;
            list.offerFirst((aCount + zCount) % 1000000007);
            list.offerFirst(zCount);
            delta = delta % 1000000007;
            delta = (delta + zCount) % 1000000007;
        }
        return delta;
    }
}
```