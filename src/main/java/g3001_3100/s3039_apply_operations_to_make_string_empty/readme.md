[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3039\. Apply Operations to Make String Empty

Medium

You are given a string `s`.

Consider performing the following operation until `s` becomes **empty**:

*   For **every** alphabet character from `'a'` to `'z'`, remove the **first** occurrence of that character in `s` (if it exists).

For example, let initially `s = "aabcbbca"`. We do the following operations:

*   Remove the underlined characters <code>s = "<ins>**a**</ins>a**<ins>bc</ins>**bbca"</code>. The resulting string is `s = "abbca"`.
*   Remove the underlined characters <code>s = "<ins>**ab**</ins>b<ins>**c**</ins>a"</code>. The resulting string is `s = "ba"`.
*   Remove the underlined characters <code>s = "<ins>**ba**</ins>"</code>. The resulting string is `s = ""`.

Return _the value of the string_ `s` _right **before** applying the **last** operation_. In the example above, answer is `"ba"`.

**Example 1:**

**Input:** s = "aabcbbca"

**Output:** "ba"

**Explanation:** Explained in the statement.

**Example 2:**

**Input:** s = "abcd"

**Output:** "abcd"

**Explanation:** We do the following operation: 
- Remove the underlined characters s = "<ins>**abcd**</ins>". The resulting string is s = "". 

The string just before the last operation is "abcd".

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>5</sup></code>
*   `s` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public String lastNonEmptyString(String s) {
        int[] freq = new int[26];
        char[] ar = s.toCharArray();
        int n = ar.length;
        int max = 1;
        StringBuilder sb = new StringBuilder();
        for (char c : ar) {
            freq[c - 'a']++;
            max = Math.max(freq[c - 'a'], max);
        }
        for (int i = n - 1; i >= 0; i--) {
            if (freq[ar[i] - 'a'] == max) {
                sb.append(ar[i]);
                freq[ar[i] - 'a'] = 0;
            }
        }
        return sb.reverse().toString();
    }
}
```