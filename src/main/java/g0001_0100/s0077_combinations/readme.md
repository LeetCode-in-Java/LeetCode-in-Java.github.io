[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 77\. Combinations

Medium

Given two integers `n` and `k`, return _all possible combinations of_ `k` _numbers out of the range_ `[1, n]`.

You may return the answer in **any order**.

**Example 1:**

**Input:** n = 4, k = 2

**Output:** [ [2,4], [3,4], [2,3], [1,2], [1,3], [1,4], ] 

**Example 2:**

**Input:** n = 1, k = 1

**Output:** [[1]] 

**Constraints:**

*   `1 <= n <= 20`
*   `1 <= k <= n`

## Solution

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

@SuppressWarnings("java:S1149")
public class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> ans = new ArrayList<>();
        // Boundary case
        if (n > 20 || k < 1 || k > n) {
            return ans;
        }
        backtrack(ans, n, k, 1, new Stack<>());
        return ans;
    }

    private void backtrack(List<List<Integer>> ans, int n, int k, int s, Stack<Integer> stack) {
        // Base case
        // If k becomes 0
        if (k == 0) {
            ans.add(new ArrayList<>(stack));
            return;
        }
        // Start with s till n-k+1
        for (int i = s; i <= (n - k) + 1; i++) {
            stack.push(i);
            // Update start for recursion and decrease k by 1
            backtrack(ans, n, k - 1, i + 1, stack);
            stack.pop();
        }
    }
}
```