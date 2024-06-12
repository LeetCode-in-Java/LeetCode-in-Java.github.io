[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3170\. Lexicographically Minimum String After Removing Stars

Medium

You are given a string `s`. It may contain any number of `'*'` characters. Your task is to remove all `'*'` characters.

While there is a `'*'`, do the following operation:

*   Delete the leftmost `'*'` and the **smallest** non-`'*'` character to its _left_. If there are several smallest characters, you can delete any of them.

Return the lexicographically smallest resulting string after removing all `'*'` characters.

**Example 1:**

**Input:** s = "aaba\*"

**Output:** "aab"

**Explanation:**

We should delete one of the `'a'` characters with `'*'`. If we choose `s[3]`, `s` becomes the lexicographically smallest.

**Example 2:**

**Input:** s = "abc"

**Output:** "abc"

**Explanation:**

There is no `'*'` in the string.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists only of lowercase English letters and `'*'`.
*   The input is generated such that it is possible to delete all `'*'` characters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public String clearStars(String s) {
        char[] arr = s.toCharArray();
        int[] idxChain = new int[arr.length];
        int[] lastIdx = new int[26];
        Arrays.fill(idxChain, -1);
        Arrays.fill(lastIdx, -1);
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == '*') {
                for (int j = 0; j < 26; j++) {
                    if (lastIdx[j] != -1) {
                        arr[lastIdx[j]] = '#';
                        lastIdx[j] = idxChain[lastIdx[j]];
                        break;
                    }
                }
                arr[i] = '#';
            } else {
                idxChain[i] = lastIdx[arr[i] - 'a'];
                lastIdx[arr[i] - 'a'] = i;
            }
        }
        StringBuilder sb = new StringBuilder();
        for (char c : arr) {
            if (c != '#') {
                sb.append(c);
            }
        }
        return sb.toString();
    }
}
```