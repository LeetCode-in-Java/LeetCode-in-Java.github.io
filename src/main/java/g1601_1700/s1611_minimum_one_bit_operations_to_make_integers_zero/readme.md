[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1611\. Minimum One Bit Operations to Make Integers Zero

Hard

Given an integer `n`, you must transform it into `0` using the following operations any number of times:

*   Change the rightmost (<code>0<sup>th</sup></code>) bit in the binary representation of `n`.
*   Change the <code>i<sup>th</sup></code> bit in the binary representation of `n` if the <code>(i-1)<sup>th</sup></code> bit is set to `1` and the <code>(i-2)<sup>th</sup></code> through <code>0<sup>th</sup></code> bits are set to `0`.

Return _the minimum number of operations to transform_ `n` _into_ `0`_._

**Example 1:**

**Input:** n = 3

**Output:** 2

**Explanation:** The binary representation of 3 is "11". 

"11" -> "01" with the 2<sup>nd</sup> operation since the 0<sup>th</sup> bit is 1. 

"01" -> "00" with the 1<sup>st</sup> operation.

**Example 2:**

**Input:** n = 6

**Output:** 4

**Explanation:** The binary representation of 6 is "110". 

"110" -> "010" with the 2<sup>nd</sup> operation since the 1<sup>st</sup> bit is 1 and 0<sup>th</sup> through 0<sup>th</sup> bits are 0. 

"010" -> "011" with the 1<sup>st</sup> operation. 

"011" -> "001" with the 2<sup>nd</sup> operation since the 0<sup>th</sup> bit is 1. 

"001" -> "000" with the 1<sup>st</sup> operation.

**Constraints:**

*   <code>0 <= n <= 10<sup>9</sup></code>

## Solution

```java
import java.util.LinkedList;

public class Solution {
    public int minimumOneBitOperations(int n) {
        return calc(calculateOneIndex(n));
    }

    private int calc(LinkedList<Integer> indices) {
        if (indices.isEmpty()) {
            return 0;
        }
        int index = indices.removeLast();
        return stepOfExp(index) - calc(indices);
    }

    private LinkedList<Integer> calculateOneIndex(int n) {
        LinkedList<Integer> result = new LinkedList<>();
        int index = 1;
        while (n > 0) {
            if (n % 2 == 1) {
                result.add(index);
            }
            n >>= 1;

            index++;
        }
        return result;
    }

    private int stepOfExp(int index) {
        int result = 1;
        while (index > 0) {
            result <<= 1;
            index--;
        }
        return result - 1;
    }
}
```