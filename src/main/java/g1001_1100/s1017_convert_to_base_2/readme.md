[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1017\. Convert to Base -2

Medium

Given an integer `n`, return _a binary string representing its representation in base_ `-2`.

**Note** that the returned string should not have leading zeros unless the string is `"0"`.

**Example 1:**

**Input:** n = 2

**Output:** "110" **Explantion:** (-2)<sup>2</sup> + (-2)<sup>1</sup> = 2

**Example 2:**

**Input:** n = 3

**Output:** "111" **Explantion:** (-2)<sup>2</sup> + (-2)<sup>1</sup> + (-2)<sup>0</sup> = 3

**Example 3:**

**Input:** n = 4

**Output:** "100" **Explantion:** (-2)<sup>2</sup> = 4

**Constraints:**

*   <code>0 <= n <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public String baseNeg2(int n) {
        StringBuilder sb = new StringBuilder(Integer.toBinaryString(n));
        sb.reverse();
        int carry = 0;
        int sum;
        int pos = 0;
        while (pos < sb.length()) {
            sum = carry + sb.charAt(pos) - '0';
            sb.setCharAt(pos, sum % 2 == 0 ? '0' : '1');
            carry = sum / 2;
            if (pos % 2 == 1 && sb.charAt(pos) == '1') {
                carry += 1;
            }
            pos++;
            if (pos >= sb.length() && carry > 0) {
                sb.append(Integer.toBinaryString(carry));
                carry = 0;
            }
        }
        return sb.reverse().toString();
    }
}
```