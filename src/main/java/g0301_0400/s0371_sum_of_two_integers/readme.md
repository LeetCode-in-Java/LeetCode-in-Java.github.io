## 371\. Sum of Two Integers

Medium

Given two integers `a` and `b`, return _the sum of the two integers without using the operators_ `+` _and_ `-`.

**Example 1:**

**Input:** a = 1, b = 2

**Output:** 3

**Example 2:**

**Input:** a = 2, b = 3

**Output:** 5

**Constraints:**

*   `-1000 <= a, b <= 1000`

## Solution

```java
public class Solution {
    public int getSum(int a, int b) {
        int ans = 0;
        int memo = 0;
        int exp = 0;
        int count = 0;
        while (count < 32) {
            int val1 = a & 1;
            int val2 = b & 1;
            int val = sum(val1, val2, memo);
            memo = val >> 1;
            val = val & 1;
            a = a >> 1;
            b = b >> 1;
            ans = ans | (val << exp);
            exp = plusOne(exp);
            count = plusOne(count);
        }
        return ans;
    }

    private int sum(int val1, int val2, int val3) {
        int count = 0;
        if (val1 == 1) {
            count = plusOne(count);
        }
        if (val2 == 1) {
            count = plusOne(count);
        }
        if (val3 == 1) {
            count = plusOne(count);
        }
        return count;
    }

    private int plusOne(int val) {
        return (-(~val));
    }
}
```