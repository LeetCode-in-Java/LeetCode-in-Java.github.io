## 679\. 24 Game

Hard

You are given an integer array `cards` of length `4`. You have four cards, each containing a number in the range `[1, 9]`. You should arrange the numbers on these cards in a mathematical expression using the operators `['+', '-', '*', '/']` and the parentheses `'('` and `')'` to get the value 24.

You are restricted with the following rules:

*   The division operator `'/'` represents real division, not integer division.
    *   For example, `4 / (1 - 2 / 3) = 4 / (1 / 3) = 12`.
*   Every operation done is between two numbers. In particular, we cannot use `'-'` as a unary operator.
    *   For example, if `cards = [1, 1, 1, 1]`, the expression `"-1 - 1 - 1 - 1"` is **not allowed**.
*   You cannot concatenate numbers together
    *   For example, if `cards = [1, 2, 1, 2]`, the expression `"12 + 12"` is not valid.

Return `true` if you can get such expression that evaluates to `24`, and `false` otherwise.

**Example 1:**

**Input:** cards = [4,1,8,7]

**Output:** true

**Explanation:** (8-4) \* (7-1) = 24

**Example 2:**

**Input:** cards = [1,2,1,2]

**Output:** false

**Constraints:**

*   `cards.length == 4`
*   `1 <= cards[i] <= 9`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static final double EPS = 1e-6;

    private boolean backtrack(double[] list, int n) {
        if (n == 1) {
            return Math.abs(list[0] - 24) < EPS;
        }
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                double a = list[i];
                double b = list[j];
                list[j] = list[n - 1];
                list[i] = a + b;
                if (backtrack(list, n - 1)) {
                    return true;
                }
                list[i] = a - b;
                if (backtrack(list, n - 1)) {
                    return true;
                }
                list[i] = b - a;
                if (backtrack(list, n - 1)) {
                    return true;
                }
                list[i] = a * b;
                if (backtrack(list, n - 1)) {
                    return true;
                }
                if (Math.abs(b) > EPS) {
                    list[i] = a / b;
                    if (backtrack(list, n - 1)) {
                        return true;
                    }
                }
                if (Math.abs(a) > EPS) {
                    list[i] = b / a;
                    if (backtrack(list, n - 1)) {
                        return true;
                    }
                }
                list[i] = a;
                list[j] = b;
            }
        }
        return false;
    }

    public boolean judgePoint24(int[] nums) {
        double[] a = Arrays.stream(nums).asDoubleStream().toArray();
        return backtrack(a, a.length);
    }
}
```