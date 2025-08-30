[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3658\. GCD of Odd and Even Sums

Easy

You are given an integer `n`. Your task is to compute the **GCD** (greatest common divisor) of two values:

*   `sumOdd`: the sum of the first `n` odd numbers.
    
*   `sumEven`: the sum of the first `n` even numbers.
    

Return the GCD of `sumOdd` and `sumEven`.

**Example 1:**

**Input:** n = 4

**Output:** 4

**Explanation:**

*   Sum of the first 4 odd numbers `sumOdd = 1 + 3 + 5 + 7 = 16`
*   Sum of the first 4 even numbers `sumEven = 2 + 4 + 6 + 8 = 20`

Hence, `GCD(sumOdd, sumEven) = GCD(16, 20) = 4`.

**Example 2:**

**Input:** n = 5

**Output:** 5

**Explanation:**

*   Sum of the first 5 odd numbers `sumOdd = 1 + 3 + 5 + 7 + 9 = 25`
*   Sum of the first 5 even numbers `sumEven = 2 + 4 + 6 + 8 + 10 = 30`

Hence, `GCD(sumOdd, sumEven) = GCD(25, 30) = 5`.

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```java
public class Solution {
    public int gcdOfOddEvenSums(int n) {
        return (n < 0) ? -n : n;
    }
}
```