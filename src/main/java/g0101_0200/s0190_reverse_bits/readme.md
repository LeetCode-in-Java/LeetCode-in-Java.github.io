[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 190\. Reverse Bits

Easy

Reverse bits of a given 32 bits signed integer.

**Example 1:**

**Input:** n = 43261596

**Output:** 964176192

**Explanation:**

Integer

Binary

43261596

00000010100101000001111010011100

964176192

00111001011110000010100101000000

**Example 2:**

**Input:** n = 2147483644

**Output:** 1073741822

**Explanation:**

Integer

Binary

2147483644

01111111111111111111111111111100

1073741822

00111111111111111111111111111110

**Constraints:**

*   <code>0 <= n <= 2<sup>31</sup> - 2</code>
*   `n` is even.

**Follow up:** If this function is called many times, how would you optimize it?

## Solution

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int ret = 0;
        // because there are 32 bits in total
        for (int i = 0; i < 32; i++) {
            ret = ret << 1;
            // If the bit is 1 we OR it with 1, ie add 1
            if ((n & 1) > 0) {
                ret = ret | 1;
            }
            n = n >>> 1;
        }
        return ret;
    }
}
```