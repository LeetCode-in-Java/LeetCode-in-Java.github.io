[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1405\. Longest Happy String

Medium

A string `s` is called **happy** if it satisfies the following conditions:

*   `s` only contains the letters `'a'`, `'b'`, and `'c'`.
*   `s` does not contain any of `"aaa"`, `"bbb"`, or `"ccc"` as a substring.
*   `s` contains **at most** `a` occurrences of the letter `'a'`.
*   `s` contains **at most** `b` occurrences of the letter `'b'`.
*   `s` contains **at most** `c` occurrences of the letter `'c'`.

Given three integers `a`, `b`, and `c`, return _the **longest possible happy** string_. If there are multiple longest happy strings, return _any of them_. If there is no such string, return _the empty string_ `""`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** a = 1, b = 1, c = 7

**Output:** "ccaccbcc"

**Explanation:** "ccbccacc" would also be a correct answer.

**Example 2:**

**Input:** a = 7, b = 1, c = 0

**Output:** "aabaa"

**Explanation:** It is the only correct answer in this case.

**Constraints:**

*   `0 <= a, b, c <= 100`
*   `a + b + c > 0`

## Solution

```java
public class Solution {
    public String longestDiverseString(int a, int b, int c) {
        StringBuilder sb = new StringBuilder();
        int[] remains = new int[] {a, b, c};
        char[] chars = new char[] {'a', 'b', 'c'};
        int preIndex = -1;
        do {
            int index;
            boolean largest;
            if (preIndex != -1
                    && remains[preIndex]
                            == Math.max(remains[0], Math.max(remains[1], remains[2]))) {
                if (preIndex == 0) {
                    index = remains[1] > remains[2] ? 1 : 2;
                } else if (preIndex == 1) {
                    index = remains[0] > remains[2] ? 0 : 2;
                } else {
                    index = remains[0] > remains[1] ? 0 : 1;
                }
                largest = false;
            } else {
                index = remains[0] > remains[1] ? 0 : 1;
                index = remains[index] > remains[2] ? index : 2;
                largest = true;
            }
            remains[index]--;
            sb.append(chars[index]);
            if (remains[index] > 0 && largest) {
                remains[index]--;
                sb.append(chars[index]);
            }
            preIndex = index;
        } while (remains[0] + remains[1] + remains[2] != remains[preIndex]);
        return sb.toString();
    }
}
```