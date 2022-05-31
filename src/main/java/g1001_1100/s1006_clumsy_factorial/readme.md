## 1006\. Clumsy Factorial

Medium

The **factorial** of a positive integer `n` is the product of all positive integers less than or equal to `n`.

*   For example, `factorial(10) = 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1`.

We make a **clumsy factorial** using the integers in decreasing order by swapping out the multiply operations for a fixed rotation of operations with multiply `'*'`, divide `'/'`, add `'+'`, and subtract `'-'` in this order.

*   For example, `clumsy(10) = 10 * 9 / 8 + 7 - 6 * 5 / 4 + 3 - 2 * 1`.

However, these operations are still applied using the usual order of operations of arithmetic. We do all multiplication and division steps before any addition or subtraction steps, and multiplication and division steps are processed left to right.

Additionally, the division that we use is floor division such that `10 * 9 / 8 = 90 / 8 = 11`.

Given an integer `n`, return _the clumsy factorial of_ `n`.

**Example 1:**

**Input:** n = 4

**Output:** 7

**Explanation:** 7 = 4 \* 3 / 2 + 1

**Example 2:**

**Input:** n = 10

**Output:** 12

**Explanation:** 12 = 10 \* 9 / 8 + 7 - 6 \* 5 / 4 + 3 - 2 \* 1

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    private int m = 1;

    public int clumsy(int n) {
        int num = 0;
        if (n >= 4) {
            num = m * n * (n - 1) / (n - 2) + (n - 3);
        } else if (n == 3) {
            num = m * n * (n - 1) / (n - 2);
        } else if (n == 2) {
            num = m * n * (n - 1);
        } else if (n == 1) {
            num = m * n;
        } else {
            return 0;
        }
        m = -1;
        return num + clumsy(n - 4);
    }
}
```