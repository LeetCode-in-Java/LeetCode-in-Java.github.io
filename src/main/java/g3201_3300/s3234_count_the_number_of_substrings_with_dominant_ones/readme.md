[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3234\. Count the Number of Substrings With Dominant Ones

Medium

You are given a binary string `s`.

Return the number of substrings with **dominant** ones.

A string has **dominant** ones if the number of ones in the string is **greater than or equal to** the **square** of the number of zeros in the string.

**Example 1:**

**Input:** s = "00011"

**Output:** 5

**Explanation:**

The substrings with dominant ones are shown in the table below.

| i | j | s[i..j] | Number of Zeros | Number of Ones |
|---|---|---------|-----------------|----------------|
| 3 | 3 | 1       | 0               | 1              |
| 4 | 4 | 1       | 0               | 1              |
| 2 | 3 | 01      | 1               | 1              |
| 3 | 4 | 11      | 0               | 2              |
| 2 | 4 | 011     | 1               | 2              |

**Example 2:**

**Input:** s = "101101"

**Output:** 16

**Explanation:**

The substrings with **non-dominant** ones are shown in the table below.

Since there are 21 substrings total and 5 of them have non-dominant ones, it follows that there are 16 substrings with dominant ones.

| i | j | s[i..j] | Number of Zeros | Number of Ones |
|---|---|---------|-----------------|----------------|
| 1 | 1 | 0       | 1               | 0              |
| 4 | 4 | 0       | 1               | 0              |
| 1 | 4 | 0110    | 2               | 2              |
| 0 | 4 | 10110   | 2               | 3              |
| 1 | 5 | 01101   | 2               | 3              |

**Constraints:**

*   <code>1 <= s.length <= 4 * 10<sup>4</sup></code>
*   `s` consists only of characters `'0'` and `'1'`.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int numberOfSubstrings(String s) {
        List<Integer> zero = new ArrayList<>();
        zero.add(-1);
        int result = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '0') {
                zero.add(i);
            } else {
                result += i - zero.get(zero.size() - 1);
            }
            for (int j = 1; j < zero.size(); j++) {
                int len = j * (j + 1);
                if (len > i + 1) {
                    break;
                }
                int prev = zero.get(zero.size() - j - 1);
                int from = Math.min(i - len + 1, zero.get(zero.size() - j));
                if (from > prev) {
                    result += from - prev;
                }
            }
        }
        return result;
    }
}
```