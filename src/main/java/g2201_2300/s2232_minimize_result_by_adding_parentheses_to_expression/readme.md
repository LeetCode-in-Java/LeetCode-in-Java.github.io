[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2232\. Minimize Result by Adding Parentheses to Expression

Medium

You are given a **0-indexed** string `expression` of the form `"<num1>+<num2>"` where `<num1>` and `<num2>` represent positive integers.

Add a pair of parentheses to `expression` such that after the addition of parentheses, `expression` is a **valid** mathematical expression and evaluates to the **smallest** possible value. The left parenthesis **must** be added to the left of `'+'` and the right parenthesis **must** be added to the right of `'+'`.

Return `expression` _after adding a pair of parentheses such that_ `expression` _evaluates to the **smallest** possible value._ If there are multiple answers that yield the same result, return any of them.

The input has been generated such that the original value of `expression`, and the value of `expression` after adding any pair of parentheses that meets the requirements fits within a signed 32-bit integer.

**Example 1:**

**Input:** expression = "247+38"

**Output:** "2(47+38)"

**Explanation:** The `expression` evaluates to 2 \* (47 + 38) = 2 \* 85 = 170.

Note that "2(4)7+38" is invalid because the right parenthesis must be to the right of the `'+'`.

It can be shown that 170 is the smallest possible value. 

**Example 2:**

**Input:** expression = "12+34"

**Output:** "1(2+3)4"

**Explanation:** The expression evaluates to 1 \* (2 + 3) \* 4 = 1 \* 5 \* 4 = 20. 

**Example 3:**

**Input:** expression = "999+999"

**Output:** "(999+999)"

**Explanation:** The `expression` evaluates to 999 + 999 = 1998. 

**Constraints:**

*   `3 <= expression.length <= 10`
*   `expression` consists of digits from `'1'` to `'9'` and `'+'`.
*   `expression` starts and ends with digits.
*   `expression` contains exactly one `'+'`.
*   The original value of `expression`, and the value of `expression` after adding any pair of parentheses that meets the requirements fits within a signed 32-bit integer.

## Solution

```java
public class Solution {
    // Variables for final solution, to avoid create combination Strings
    private int currentLeft = 0;
    private int currentRight = 0;
    private int currentMin = Integer.MAX_VALUE;

    public String minimizeResult(String expression) {
        // Identify our starting point, to apply the expansion technique
        int plusIndex = expression.indexOf("+");
        // We start expanding from the first values to the left and right of the center (plus sign).
        expand(plusIndex - 1, plusIndex + 1, expression);
        // Build the final String. We add the parentheses to our expression in the already
        // calculated indices, defined as global variables.
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < expression.length(); i++) {
            if (i == currentLeft) {
                stringBuilder.append('(');
            }
            stringBuilder.append(expression.charAt(i));
            if (i == currentRight) {
                stringBuilder.append(')');
            }
        }
        return stringBuilder.toString();
    }

    // With this function, we calculate all possible combinations of parentheses from two pointers,
    // left and right.
    private void expand(int left, int right, String expression) {
        if (left < 0 || right >= expression.length()) {
            return;
        }
        // from zero to first parentheses
        int a = evaluate(0, left, expression);
        // from first parentheses to right parentheses (+1 means inclusive)
        int b = evaluate(left, right + 1, expression);
        // from right parentheses to the end of expression (+1 means inclusive)
        int c = evaluate(right + 1, expression.length(), expression);
        // If the expression a * b * c is less than our current minimum
        // this is our solution, we replace the variables with these new values.
        if ((a * b * c) < currentMin) {
            currentMin = a * b * c;
            currentLeft = left;
            currentRight = right;
        }
        // Move parentheses left only
        expand(left - 1, right, expression);
        // Move parentheses right only
        expand(left, right + 1, expression);
        // Move parentheses left and right
        expand(left - 1, right + 1, expression);
    }

    /* This function is responsible for calculating the expressions of each variable.

    a = (0, left) // from the start of the expression to the first parentheses
    b = (left, right) // between parentheses, include plus sign
    c = (right, end of expression) // from the last parentheses to the end
    */
    private int evaluate(int left, int right, String expression) {
        // This means that the parentheses are at the beginning or end of the expression and are
        // equal to the range of the expression to be evaluated. Return 1 to avoid zero factors in
        // equation (a * b * c).
        if (left == right) {
            return 1;
        }
        int number = 0;
        for (int i = left; i < right; i++) {
            // If we find a sign, we must add both parts, therefore, we convert the expression to (a
            // + b).
            // We return the variable (a) wich is (number) and add to what follows after the sign (i
            // + 1).
            // We call the same function to calculate the b value.
            if (expression.charAt(i) == '+') {
                return number + evaluate(i + 1, right, expression);
            } else {
                number = (number * 10) + (expression.charAt(i) - '0');
            }
        }
        return number;
    }
}
```