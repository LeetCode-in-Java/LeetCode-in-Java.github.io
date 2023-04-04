[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1009\. Complement of Base 10 Integer

Easy

The **complement** of an integer is the integer you get when you flip all the `0`'s to `1`'s and all the `1`'s to `0`'s in its binary representation.

*   For example, The integer `5` is `"101"` in binary and its **complement** is `"010"` which is the integer `2`.

Given an integer `n`, return _its complement_.

**Example 1:**

**Input:** n = 5

**Output:** 2

**Explanation:** 5 is "101" in binary, with complement "010" in binary, which is 2 in base-10.

**Example 2:**

**Input:** n = 7

**Output:** 0

**Explanation:** 7 is "111" in binary, with complement "000" in binary, which is 0 in base-10.

**Example 3:**

**Input:** n = 10

**Output:** 5

**Explanation:** 10 is "1010" in binary, with complement "0101" in binary, which is 5 in base-10.

**Constraints:**

*   <code>0 <= n < 10<sup>9</sup></code>

**Note:** This question is the same as 476: [https://leetcode.com/problems/number-complement/](https://leetcode.com/problems/number-complement/)

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int bitwiseComplement(int n) {
        if (n == 0) {
            return 1;
        }
        List<Integer> list = new ArrayList<>();
        while (n != 0) {
            list.add(n & 1);
            n >>= 1;
        }
        int result = 0;
        int exp = list.size() - 1;
        for (int i = list.size() - 1; i >= 0; i--) {
            if (list.get(i) == 0) {
                result += Math.pow(2, exp);
            }
            exp--;
        }
        return result;
    }
}
```