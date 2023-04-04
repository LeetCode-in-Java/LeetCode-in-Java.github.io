[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 640\. Solve the Equation

Medium

Solve a given equation and return the value of `'x'` in the form of a string `"x=#value"`. The equation contains only `'+'`, `'-'` operation, the variable `'x'` and its coefficient. You should return `"No solution"` if there is no solution for the equation, or `"Infinite solutions"` if there are infinite solutions for the equation.

If there is exactly one solution for the equation, we ensure that the value of `'x'` is an integer.

**Example 1:**

**Input:** equation = "x+5-3+x=6+x-2"

**Output:** "x=2"

**Example 2:**

**Input:** equation = "x=x"

**Output:** "Infinite solutions"

**Example 3:**

**Input:** equation = "2x=x"

**Output:** "x=0"

**Constraints:**

*   `3 <= equation.length <= 1000`
*   `equation` has exactly one `'='`.
*   `equation` consists of integers with an absolute value in the range `[0, 100]` without any leading zeros, and the variable `'x'`.

## Solution

```java
public class Solution {
    public String solveEquation(String equation) {
        String[] eqs = equation.split("=");

        int[] arr1 = evaluate(eqs[0]);
        int[] arr2 = evaluate(eqs[1]);

        if (arr1[0] == arr2[0] && arr1[1] == arr2[1]) {
            return "Infinite solutions";
        } else if (arr1[0] == arr2[0]) {
            return "No solution";
        } else {
            return "x=" + (arr2[1] - arr1[1]) / (arr1[0] - arr2[0]);
        }
    }

    private int[] evaluate(String eq) {
        char[] arr = eq.toCharArray();
        boolean f = false;
        int a = 0;
        int b = 0;

        int i = 0;
        if (arr[0] == '-') {
            f = true;
            i++;
        }
        while (i < arr.length) {
            if (arr[i] == '-') {
                f = true;
                i++;
            } else if (arr[i] == '+') {
                i++;
            }
            StringBuilder sb = new StringBuilder();
            while (i < arr.length && Character.isDigit(arr[i])) {
                sb.append(arr[i]);
                i++;
            }
            String n = sb.toString();
            if (i < arr.length && arr[i] == 'x') {
                int number;
                if (n.equals("")) {
                    number = 1;
                } else {
                    number = Integer.parseInt(n);
                }
                if (f) {
                    number = -number;
                }
                a += number;
                i++;
            } else {
                int number = Integer.parseInt(n);
                if (f) {
                    number = -number;
                }
                b += number;
            }
            f = false;
        }
        int[] op = new int[2];
        op[0] = a;
        op[1] = b;
        return op;
    }
}
```