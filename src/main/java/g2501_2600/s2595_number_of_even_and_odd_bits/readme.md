[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2595\. Number of Even and Odd Bits

Easy

You are given a **positive** integer `n`.

Let `even` denote the number of even indices in the binary representation of `n` (**0-indexed**) with value `1`.

Let `odd` denote the number of odd indices in the binary representation of `n` (**0-indexed**) with value `1`.

Return _an integer array_ `answer` _where_ `answer = [even, odd]`.

**Example 1:**

**Input:** n = 17

**Output:** [2,0]

**Explanation:**

The binary representation of 17 is 10001.

It contains 1 on the 0<sup>th</sup> and 4<sup>th</sup> indices.

There are 2 even and 0 odd indices.

**Example 2:**

**Input:** n = 2

**Output:** [0,1]

**Explanation:**

The binary representation of 2 is 10.

It contains 1 on the 1<sup>st</sup> index.

There are 0 even and 1 odd indices.

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```java
public class Solution {
    public int[] evenOddBit(int n) {
        int[] res = new int[2];
        int even = 0;
        int odd = 0;
        for (int i = 0; i < 32; i++) {
            if (i % 2 == 0) {
                if ((n & 1) == 1) {
                    even++;
                }
            } else {
                if ((n & 1) == 1) {
                    odd++;
                }
            }
            n >>= 1;
        }
        res[0] = even;
        res[1] = odd;
        return res;
    }
}
```