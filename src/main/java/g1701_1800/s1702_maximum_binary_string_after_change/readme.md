[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1702\. Maximum Binary String After Change

Medium

You are given a binary string `binary` consisting of only `0`'s or `1`'s. You can apply each of the following operations any number of times:

*   Operation 1: If the number contains the substring `"00"`, you can replace it with `"10"`.
    *   For example, `"00010" -> "10010`"
*   Operation 2: If the number contains the substring `"10"`, you can replace it with `"01"`.
    *   For example, `"00010" -> "00001"`

_Return the **maximum binary string** you can obtain after any number of operations. Binary string `x` is greater than binary string `y` if `x`'s decimal representation is greater than `y`'s decimal representation._

**Example 1:**

**Input:** binary = "000110"

**Output:** "111011"

**Explanation:** A valid transformation sequence can be: 

"000110" -> "000101"

"000101" -> "100101" 

"100101" -> "110101" 

"110101" -> "110011" 

"110011" -> "111011"

**Example 2:**

**Input:** binary = "01"

**Output:** "01"

**Explanation:** "01" cannot be transformed any further.

**Constraints:**

*   <code>1 <= binary.length <= 10<sup>5</sup></code>
*   `binary` consist of `'0'` and `'1'`.

## Solution

```java
public class Solution {
    public String maximumBinaryString(String binary) {
        char[] bs = binary.toCharArray();
        int zcount = 0;
        int pos = -1;
        for (int i = bs.length - 1; i >= 0; i--) {
            if (bs[i] == '0') {
                bs[i] = '1';
                zcount++;
                pos = i;
            }
        }
        if (pos >= 0) {
            bs[pos + zcount - 1] = '0';
        }
        return new String(bs);
    }
}
```