[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3816\. Lexicographically Smallest String After Deleting Duplicate Characters

Hard

You are given a string `s` that consists of lowercase English letters.

You can perform the following operation any number of times (possibly zero times):

*   Choose any letter that appears **at least twice** in the current string `s` and delete any **one** occurrence.

Return the **lexicographically smallest** resulting string that can be formed this way.

**Example 1:**

**Input:** s = "aaccb"

**Output:** "aacb"

**Explanation:**

We can form the strings `"acb"`, `"aacb"`, `"accb"`, and `"aaccb"`. `"aacb"` is the lexicographically smallest one.

For example, we can obtain `"aacb"` by choosing `'c'` and deleting its first occurrence.

**Example 2:**

**Input:** s = "z"

**Output:** "z"

**Explanation:**

We cannot perform any operations. The only string we can form is `"z"`.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` contains lowercase English letters only.

## Solution

```java
public class Solution {
    public String lexSmallestAfterDeletion(String s) {
        char[] a = s.toCharArray();
        int[] rem = new int[26];
        for (char c : a) {
            rem[c - 'a']++;
        }
        int[] cnt = new int[26];
        StringBuilder sb = new StringBuilder();
        for (char c : a) {
            rem[c - 'a']--;
            while (!sb.isEmpty()) {
                char t = sb.charAt(sb.length() - 1);
                if (t > c && (rem[t - 'a'] > 0 || cnt[t - 'a'] > 1)) {
                    cnt[t - 'a']--;
                    sb.setLength(sb.length() - 1);
                } else {
                    break;
                }
            }
            sb.append(c);
            cnt[c - 'a']++;
        }
        while (!sb.isEmpty()) {
            int i = sb.length() - 1;
            char c = sb.charAt(i);
            if (cnt[c - 'a'] > 1) {
                cnt[c - 'a']--;
                sb.setLength(i);
            } else {
                break;
            }
        }
        return sb.toString();
    }
}
```