[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3228\. Maximum Number of Operations to Move Ones to the End

Medium

You are given a binary string `s`.

You can perform the following operation on the string **any** number of times:

*   Choose **any** index `i` from the string where `i + 1 < s.length` such that `s[i] == '1'` and `s[i + 1] == '0'`.
*   Move the character `s[i]` to the **right** until it reaches the end of the string or another `'1'`. For example, for `s = "010010"`, if we choose `i = 1`, the resulting string will be <code>s = "0**<ins>001</ins>**10"</code>.

Return the **maximum** number of operations that you can perform.

**Example 1:**

**Input:** s = "1001101"

**Output:** 4

**Explanation:**

We can perform the following operations:

*   Choose index `i = 0`. The resulting string is <code>s = "<ins>**001**</ins>1101"</code>.
*   Choose index `i = 4`. The resulting string is <code>s = "0011<ins>**01**</ins>1"</code>.
*   Choose index `i = 3`. The resulting string is <code>s = "001**<ins>01</ins>**11"</code>.
*   Choose index `i = 2`. The resulting string is <code>s = "00**<ins>01</ins>**111"</code>.

**Example 2:**

**Input:** s = "00111"

**Output:** 0

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.

## Solution

```java
public class Solution {
    public int maxOperations(String s) {
        char[] arr = s.toCharArray();
        int result = 0;
        int ones = 0;
        int n = arr.length;
        for (int i = 0; i < n; ++i) {
            ones += arr[i] - '0';
            if (i > 0 && arr[i] < arr[i - 1]) {
                result += ones;
            }
        }
        return result;
    }
}
```