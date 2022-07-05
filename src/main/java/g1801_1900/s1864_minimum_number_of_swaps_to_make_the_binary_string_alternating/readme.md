[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1864\. Minimum Number of Swaps to Make the Binary String Alternating

Medium

Given a binary string `s`, return _the **minimum** number of character swaps to make it **alternating**, or_ `-1` _if it is impossible._

The string is called **alternating** if no two adjacent characters are equal. For example, the strings `"010"` and `"1010"` are alternating, while the string `"0100"` is not.

Any two characters may be swapped, even if they are **not adjacent**.

**Example 1:**

**Input:** s = "111000"

**Output:** 1

**Explanation:** Swap positions 1 and 4: "111000" -> "101010" The string is now alternating.

**Example 2:**

**Input:** s = "010"

**Output:** 0

**Explanation:** The string is already alternating, no swaps are needed.

**Example 3:**

**Input:** s = "1110"

**Output:** -1

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s[i]` is either `'0'` or `'1'`.

## Solution

```java
public class Solution {
    public int minSwaps(String s) {
        int[][] count = new int[2][2];
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (i % 2 == 0) {
                count[0][c - '0']++;
            } else {
                count[1][c - '0']++;
            }
        }
        if ((count[0][0] == 0 && count[1][1] == 0) || (count[0][1] == 0 && count[1][0] == 0)) {
            return 0;
        }
        if (count[0][0] != count[1][1] && count[0][1] != count[1][0]) {
            return -1;
        }
        int ans1 = count[0][0] == count[1][1] ? count[0][0] : Integer.MAX_VALUE;
        int ans2 = count[0][1] == count[1][0] ? count[0][1] : Integer.MAX_VALUE;
        return Math.min(ans1, ans2);
    }
}
```