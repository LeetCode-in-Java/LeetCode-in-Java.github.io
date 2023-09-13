[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 96\. Unique Binary Search Trees

Medium

Given an integer `n`, return _the number of structurally unique **BST'**s (binary search trees) which has exactly_ `n` _nodes of unique values from_ `1` _to_ `n`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

**Input:** n = 3

**Output:** 5 

**Example 2:**

**Input:** n = 1

**Output:** 1 

**Constraints:**

*   `1 <= n <= 19`

## Solution

```java
public class Solution {
    public int numTrees(int n) {
        long result = 1;
        for (int i = 0; i < n; i++) {
            result *= 2L * n - i;
            result /= i + 1;
        }
        result /= n + 1;
        return (int) result;
    }
}
```

**Time Complexity (Big O Time):**

The program uses a loop that iterates from 0 to 'n' to calculate the number of unique binary search trees. Inside the loop, basic arithmetic operations (multiplication and division) are performed.

- The loop runs 'n' times, so the time complexity of the loop itself is O(n).

- Inside the loop, there are simple arithmetic operations involving long integers (multiplication and division), which are constant-time operations.

- Therefore, the overall time complexity of the program is O(n), where 'n' is the input value.

**Space Complexity (Big O Space):**

The program uses a single long variable (`result`) to store the intermediate and final results. It does not use any data structures that depend on the input size 'n'.

- Regardless of the input value 'n', the space used by the program remains constant.

- Therefore, the space complexity of the program is O(1), indicating constant space usage.

In summary, the time complexity of the program is O(n), and the space complexity is O(1), where 'n' is the input value. The time complexity is determined by the loop that runs 'n' times, and the space complexity is constant because the program uses only a fixed amount of memory regardless of the input size.
