## 738\. Monotone Increasing Digits

Medium

An integer has **monotone increasing digits** if and only if each pair of adjacent digits `x` and `y` satisfy `x <= y`.

Given an integer `n`, return _the largest number that is less than or equal to_ `n` _with **monotone increasing digits**_.

**Example 1:**

**Input:** n = 10

**Output:** 9

**Example 2:**

**Input:** n = 1234

**Output:** 1234

**Example 3:**

**Input:** n = 332

**Output:** 299

**Constraints:**

*   <code>0 <= n <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int monotoneIncreasingDigits(int n) {
        for (int i = 10; n / i > 0; i *= 10) {
            int digit = (n / i) % 10;
            int endnum = n % i;
            int firstendnum = endnum * 10 / i;
            if (digit > firstendnum) {
                n -= endnum + 1;
            }
        }
        return n;
    }
}
```