[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2696\. Minimum String Length After Removing Substrings

Easy

You are given a string `s` consisting only of **uppercase** English letters.

You can apply some operations to this string where, in one operation, you can remove **any** occurrence of one of the substrings `"AB"` or `"CD"` from `s`.

Return _the **minimum** possible length of the resulting string that you can obtain_.

**Note** that the string concatenates after removing the substring and could produce new `"AB"` or `"CD"` substrings.

**Example 1:**

**Input:** s = "ABFCACDB"

**Output:** 2

**Explanation:** We can do the following operations: 
- Remove the substring "<ins>AB</ins>FCACDB", so s = "FCACDB". 
- Remove the substring "FCA<ins>CD</ins>B", so s = "FCAB". 
- Remove the substring "FC<ins>AB</ins>", so s = "FC". 

So the resulting length of the string is 2. 

It can be shown that it is the minimum length that we can obtain.

**Example 2:**

**Input:** s = "ACBBD"

**Output:** 5

**Explanation:** We cannot do any operations on the string so the length remains the same.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists only of uppercase English letters.

## Solution

```java
import java.util.ArrayDeque;

public class Solution {
    public int minLength(String s) {
        ArrayDeque<Character> stack = new ArrayDeque<>();
        for (char c : s.toCharArray()) {
            if (!stack.isEmpty()
                    && ((c == 'B' && stack.getLast() == 'A')
                            || (c == 'D' && stack.getLast() == 'C'))) {
                stack.removeLast();
            } else {
                stack.addLast(c);
            }
        }
        return stack.size();
    }
}
```