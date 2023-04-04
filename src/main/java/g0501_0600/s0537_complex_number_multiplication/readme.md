[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 537\. Complex Number Multiplication

Medium

A [complex number](https://en.wikipedia.org/wiki/Complex_number) can be represented as a string on the form `"**real**+**imaginary**i"` where:

*   `real` is the real part and is an integer in the range `[-100, 100]`.
*   `imaginary` is the imaginary part and is an integer in the range `[-100, 100]`.
*   <code>i<sup>2</sup> == -1</code>.

Given two complex numbers `num1` and `num2` as strings, return _a string of the complex number that represents their multiplications_.

**Example 1:**

**Input:** num1 = "1+1i", num2 = "1+1i"

**Output:** "0+2i"

**Explanation:** (1 + i) \* (1 + i) = 1 + i2 + 2 \* i = 2i, and you need convert it to the form of 0+2i.

**Example 2:**

**Input:** num1 = "1+-1i", num2 = "1+-1i"

**Output:** "0+-2i"

**Explanation:** (1 - i) \* (1 - i) = 1 + i2 - 2 \* i = -2i, and you need convert it to the form of 0+-2i.

**Constraints:**

*   `num1` and `num2` are valid complex numbers.

## Solution

```java
public class Solution {
    public String complexNumberMultiply(String num1, String num2) {
        int countReal;
        int countImagine;
        int[] arr1 = new int[2];
        int[] arr2 = new int[2];

        arr1[0] = Integer.parseInt(num1.substring(0, num1.indexOf("+")));
        arr1[1] = Integer.parseInt(num1.substring(num1.indexOf("+") + 1, num1.length() - 1));

        arr2[0] = Integer.parseInt(num2.substring(0, num2.indexOf("+")));
        arr2[1] = Integer.parseInt(num2.substring(num2.indexOf("+") + 1, num2.length() - 1));

        countReal = arr1[0] * arr2[0] - arr1[1] * arr2[1];
        countImagine = arr1[0] * arr2[1] + arr1[1] * arr2[0];

        return countReal + "+" + countImagine + "i";
    }
}
```