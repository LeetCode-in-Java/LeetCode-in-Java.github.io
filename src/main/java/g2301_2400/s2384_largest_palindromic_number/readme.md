[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2384\. Largest Palindromic Number

Medium

You are given a string `num` consisting of digits only.

Return _the **largest palindromic** integer (in the form of a string) that can be formed using digits taken from_ `num`. It should not contain **leading zeroes**.

**Notes:**

*   You do **not** need to use all the digits of `num`, but you must use **at least** one digit.
*   The digits can be reordered.

**Example 1:**

**Input:** num = "444947137"

**Output:** "7449447"

**Explanation:**

Use the digits "4449477" from "<ins>**44494**</ins><ins>**7**</ins>13<ins>**7**</ins>" to form the palindromic integer "7449447".

It can be shown that "7449447" is the largest palindromic integer that can be formed.

**Example 2:**

**Input:** num = "00009"

**Output:** "9"

**Explanation:**

It can be shown that "9" is the largest palindromic integer that can be formed.

Note that the integer returned should not contain leading zeroes.

**Constraints:**

*   <code>1 <= num.length <= 10<sup>5</sup></code>
*   `num` consists of digits.

## Solution

```java
public class Solution {
    public String largestPalindromic(String num) {
        int[] count = new int[10];
        int center = -1;
        StringBuilder first = new StringBuilder();
        StringBuilder second;
        for (char c : num.toCharArray()) {
            count[c - '0']++;
        }
        int c = 0;
        for (int i = 9; i >= 0; i--) {
            c = 0;
            if (count[i] % 2 == 1 && center == -1) {
                center = i;
            }
            if (first.length() == 0 && i == 0) {
                continue;
            }
            while (c < count[i] / 2) {
                first.append(String.valueOf(i));
                c++;
            }
        }
        second = new StringBuilder(first.toString());
        if (center != -1) {
            first.append(center);
        }
        first.append(second.reverse().toString());
        return first.length() == 0 ? "0" : first.toString();
    }
}
```