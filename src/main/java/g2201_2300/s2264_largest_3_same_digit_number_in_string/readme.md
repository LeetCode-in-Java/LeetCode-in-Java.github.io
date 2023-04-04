[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2264\. Largest 3-Same-Digit Number in String

Easy

You are given a string `num` representing a large integer. An integer is **good** if it meets the following conditions:

*   It is a **substring** of `num` with length `3`.
*   It consists of only one unique digit.

Return _the **maximum good** integer as a **string** or an empty string_ `""` _if no such integer exists_.

Note:

*   A **substring** is a contiguous sequence of characters within a string.
*   There may be **leading zeroes** in `num` or a good integer.

**Example 1:**

**Input:** num = "6**777**133339"

**Output:** "777"

**Explanation:** There are two distinct good integers: "777" and "333".

"777" is the largest, so we return "777". 

**Example 2:**

**Input:** num = "23**000**19"

**Output:** "000"

**Explanation:** "000" is the only good integer.

**Example 3:**

**Input:** num = "42352338"

**Output:** ""

**Explanation:** No substring of length 3 consists of only one unique digit. Therefore, there are no good integers.

**Constraints:**

*   `3 <= num.length <= 1000`
*   `num` only consists of digits.

## Solution

```java
public class Solution {
    public String largestGoodInteger(String num) {
        String maxi = "000";
        int c = 0;
        for (int i = 0; i < num.length() - 2; i++) {
            String s = num.substring(i, i + 3);
            if (s.charAt(0) == s.charAt(1) && s.charAt(1) == s.charAt(2)) {
                if (s.compareTo(maxi) >= 0) {
                    maxi = s;
                }
                ++c;
            }
        }
        if (c == 0) {
            return "";
        }
        return maxi;
    }
}
```