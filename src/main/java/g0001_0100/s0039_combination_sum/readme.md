[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 39\. Combination Sum

Medium

Given an array of **distinct** integers `candidates` and a target integer `target`, return _a list of all **unique combinations** of_ `candidates` _where the chosen numbers sum to_ `target`_._ You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is **guaranteed** that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

**Input:** candidates = [2,3,6,7], target = 7

**Output:** [[2,2,3],[7]]

**Explanation:**

    2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
    7 is a candidate, and 7 = 7.
    These are the only two combinations. 

**Example 2:**

**Input:** candidates = [2,3,5], target = 8

**Output:** [[2,2,2,2],[2,3,3],[3,5]] 

**Example 3:**

**Input:** candidates = [2], target = 1

**Output:** [] 

**Example 4:**

**Input:** candidates = [1], target = 1

**Output:** [[1]] 

**Example 5:**

**Input:** candidates = [1], target = 2

**Output:** [[1,1]] 

**Constraints:**

*   `1 <= candidates.length <= 30`
*   `1 <= candidates[i] <= 200`
*   All elements of `candidates` are **distinct**.
*   `1 <= target <= 500`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> combinationSum(int[] coins, int amount) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> subList = new ArrayList<>();
        combinationSumRec(coins.length, coins, amount, subList, ans);
        return ans;
    }

    private void combinationSumRec(
            int n, int[] coins, int amount, List<Integer> subList, List<List<Integer>> ans) {
        if (amount == 0 || n == 0) {
            if (amount == 0) {
                List<Integer> base = new ArrayList<>(subList);
                ans.add(base);
            }
            return;
        }
        if (amount - coins[n - 1] >= 0) {
            subList.add(coins[n - 1]);
            combinationSumRec(n, coins, amount - coins[n - 1], subList, ans);
            subList.remove(subList.size() - 1);
        }
        combinationSumRec(n - 1, coins, amount, subList, ans);
    }
}
```

**Time Complexity (Big O Time):**

1. The program uses a recursive function, `combinationSumRec`, to generate combinations. In the worst case, it explores all possible combinations.

2. For each coin, it has two choices: either include the coin in the combination or exclude it. This results in a binary tree-like structure for the recursive calls, where each level of the tree represents a different coin and has two branches for including or excluding the coin.

3. In the worst case, the binary tree has a height of n (the number of coins). This means that the program makes 2^n recursive calls.

4. In each recursive call, the program performs constant-time operations to add or remove elements from the `subList`, which can be considered O(1).

5. Therefore, the overall time complexity is O(2^n), where n is the number of coins.

**Space Complexity (Big O Space):**

1. The space complexity of the program is determined by the space used for the call stack during the recursion and the space used for the `subList` and `ans` list.

2. The maximum depth of the recursion is n (the number of coins), which means the space used for the call stack is O(n).

3. The `subList` stores the current combination, and its size is bounded by the number of coins. In the worst case, it can have n elements.

4. The `ans` list stores all valid combinations, and its size is determined by the number of valid combinations. In the worst case, it can also have a size of 2^n, considering all possible combinations.

5. Therefore, the space complexity of the program is O(n + 2^n), where n is the number of coins.

In summary, the time complexity of the provided program is O(2^n), and the space complexity is O(n + 2^n), where n is the number of coins.
