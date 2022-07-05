[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1432\. Max Difference You Can Get From Changing an Integer

Medium

You are given an integer `num`. You will apply the following steps exactly **two** times:

*   Pick a digit `x (0 <= x <= 9)`.
*   Pick another digit `y (0 <= y <= 9)`. The digit `y` can be equal to `x`.
*   Replace all the occurrences of `x` in the decimal representation of `num` by `y`.
*   The new integer **cannot** have any leading zeros, also the new integer **cannot** be 0.

Let `a` and `b` be the results of applying the operations to `num` the first and second times, respectively.

Return _the max difference_ between `a` and `b`.

**Example 1:**

**Input:** num = 555

**Output:** 888

**Explanation:** The first time pick x = 5 and y = 9 and store the new integer in a. 

The second time pick x = 5 and y = 1 and store the new integer in b. 

We have now a = 999 and b = 111 and max difference = 888

**Example 2:**

**Input:** num = 9

**Output:** 8

**Explanation:** The first time pick x = 9 and y = 9 and store the new integer in a. 

The second time pick x = 9 and y = 1 and store the new integer in b.

We have now a = 9 and b = 1 and max difference = 8

**Constraints:**

*   `1 <= num <= 10`<sup>8</sup>

## Solution

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Solution {
    public int maxDiff(int num) {
        Deque<Integer> stack = new ArrayDeque<>();
        int xMax = 9;
        int yMax = 9;
        int xMin = 0;
        int yMin = 0;
        int min = 0;
        int max = 0;
        boolean areDigitsUnique = true;
        while (num != 0) {
            if (!stack.isEmpty() && num % 10 != stack.peek()) {
                areDigitsUnique = false;
            }
            stack.push(num % 10);
            num /= 10;
            if (stack.peek() != 9) {
                xMax = stack.peek();
            }
            if (stack.peek() > 1) {
                xMin = stack.peek();
            }
        }
        if (areDigitsUnique || stack.peek() == xMin) {
            // Handles no leading zeros/non zero constraints.
            yMin = 1;
        }
        while (!stack.isEmpty()) {
            min = min * 10 + (stack.peek() == xMin ? yMin : stack.peek());
            max = max * 10 + (stack.peek() == xMax ? yMax : stack.peek());
            stack.pop();
        }
        return max - min;
    }
}
```