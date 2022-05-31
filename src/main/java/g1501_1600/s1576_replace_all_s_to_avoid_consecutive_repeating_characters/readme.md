## 1576\. Replace All ?'s to Avoid Consecutive Repeating Characters

Easy

Given a string `s` containing only lowercase English letters and the `'?'` character, convert **all** the `'?'` characters into lowercase letters such that the final string does not contain any **consecutive repeating** characters. You **cannot** modify the non `'?'` characters.

It is **guaranteed** that there are no consecutive repeating characters in the given string **except** for `'?'`.

Return _the final string after all the conversions (possibly zero) have been made_. If there is more than one solution, return **any of them**. It can be shown that an answer is always possible with the given constraints.

**Example 1:**

**Input:** s = "?zs"

**Output:** "azs"

**Explanation:** There are 25 solutions for this problem. From "azs" to "yzs", all are valid. Only "z" is an invalid modification as the string will consist of consecutive repeating characters in "zzs".

**Example 2:**

**Input:** s = "ubv?w"

**Output:** "ubvaw"

**Explanation:** There are 24 solutions for this problem. Only "v" and "w" are invalid modifications as the strings will consist of consecutive repeating characters in "ubvvw" and "ubvww".

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consist of lowercase English letters and `'?'`.

## Solution

```java
public class Solution {
    public String modifyString(String s) {
        StringBuilder sb = new StringBuilder();
        int len = s.length();
        for (int i = 0; i < len; i++) {
            char c = s.charAt(i);
            if (c == '?') {
                char replaceChar = 'a';
                char leftChar = i == 0 ? s.charAt(i) : sb.charAt(i - 1);
                char rightChar = s.charAt(Math.min(i + 1, len - 1));
                while (replaceChar == leftChar || replaceChar == rightChar) {
                    replaceChar += 1;
                }
                sb.append(replaceChar);
            } else {
                sb.append(c);
            }
        }
        return sb.toString();
    }
}
```