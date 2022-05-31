## 1837\. Sum of Digits in Base K

Easy

Given an integer `n` (in base `10`) and a base `k`, return _the **sum** of the digits of_ `n` _**after** converting_ `n` _from base_ `10` _to base_ `k`.

After converting, each digit should be interpreted as a base `10` number, and the sum should be returned in base `10`.

**Example 1:**

**Input:** n = 34, k = 6

**Output:** 9

**Explanation:** 34 (base 10) expressed in base 6 is 54. 5 + 4 = 9.

**Example 2:**

**Input:** n = 10, k = 10

**Output:** 1

**Explanation:** n is already in base 10. 1 + 0 = 1.

**Constraints:**

*   `1 <= n <= 100`
*   `2 <= k <= 10`

## Solution

```java
public class Solution {
    public int sumBase(int n, int k) {
        String str = Integer.toString(Integer.parseInt(n + "", 10), k);
        int sum = 0;
        for (char c : str.toCharArray()) {
            sum += Character.getNumericValue(c);
        }
        return sum;
    }
}
```