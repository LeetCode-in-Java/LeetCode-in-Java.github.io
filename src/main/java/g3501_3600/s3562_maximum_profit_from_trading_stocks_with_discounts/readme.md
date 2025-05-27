[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3562\. Maximum Profit from Trading Stocks with Discounts

Hard

You are given an integer `n`, representing the number of employees in a company. Each employee is assigned a unique ID from 1 to `n`, and employee 1 is the CEO. You are given two **1-based** integer arrays, `present` and `future`, each of length `n`, where:

*   `present[i]` represents the **current** price at which the <code>i<sup>th</sup></code> employee can buy a stock today.
*   `future[i]` represents the **expected** price at which the <code>i<sup>th</sup></code> employee can sell the stock tomorrow.

The company's hierarchy is represented by a 2D integer array `hierarchy`, where <code>hierarchy[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> means that employee <code>u<sub>i</sub></code> is the direct boss of employee <code>v<sub>i</sub></code>.

Additionally, you have an integer `budget` representing the total funds available for investment.

However, the company has a discount policy: if an employee's direct boss purchases their own stock, then the employee can buy their stock at **half** the original price (`floor(present[v] / 2)`).

Return the **maximum** profit that can be achieved without exceeding the given budget.

**Note:**

*   You may buy each stock at most **once**.
*   You **cannot** use any profit earned from future stock prices to fund additional investments and must buy only from `budget`.

**Example 1:**

**Input:** n = 2, present = [1,2], future = [4,3], hierarchy = \[\[1,2]], budget = 3

**Output:** 5

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/09/screenshot-2025-04-10-at-053641.png)

*   Employee 1 buys the stock at price 1 and earns a profit of `4 - 1 = 3`.
*   Since Employee 1 is the direct boss of Employee 2, Employee 2 gets a discounted price of `floor(2 / 2) = 1`.
*   Employee 2 buys the stock at price 1 and earns a profit of `3 - 1 = 2`.
*   The total buying cost is `1 + 1 = 2 <= budget`. Thus, the maximum total profit achieved is `3 + 2 = 5`.

**Example 2:**

**Input:** n = 2, present = [3,4], future = [5,8], hierarchy = \[\[1,2]], budget = 4

**Output:** 4

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/09/screenshot-2025-04-10-at-053641.png)

*   Employee 2 buys the stock at price 4 and earns a profit of `8 - 4 = 4`.
*   Since both employees cannot buy together, the maximum profit is 4.

**Example 3:**

**Input:** n = 3, present = [4,6,8], future = [7,9,11], hierarchy = \[\[1,2],[1,3]], budget = 10

**Output:** 10

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/09/image.png)

*   Employee 1 buys the stock at price 4 and earns a profit of `7 - 4 = 3`.
*   Employee 3 would get a discounted price of `floor(8 / 2) = 4` and earns a profit of `11 - 4 = 7`.
*   Employee 1 and Employee 3 buy their stocks at a total cost of `4 + 4 = 8 <= budget`. Thus, the maximum total profit achieved is `3 + 7 = 10`.

**Example 4:**

**Input:** n = 3, present = [5,2,3], future = [8,5,6], hierarchy = \[\[1,2],[2,3]], budget = 7

**Output:** 12

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/04/09/screenshot-2025-04-10-at-054114.png)

*   Employee 1 buys the stock at price 5 and earns a profit of `8 - 5 = 3`.
*   Employee 2 would get a discounted price of `floor(2 / 2) = 1` and earns a profit of `5 - 1 = 4`.
*   Employee 3 would get a discounted price of `floor(3 / 2) = 1` and earns a profit of `6 - 1 = 5`.
*   The total cost becomes `5 + 1 + 1 = 7 <= budget`. Thus, the maximum total profit achieved is `3 + 4 + 5 = 12`.

**Constraints:**

*   `1 <= n <= 160`
*   `present.length, future.length == n`
*   `1 <= present[i], future[i] <= 50`
*   `hierarchy.length == n - 1`
*   <code>hierarchy[i] == [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>1 <= u<sub>i</sub>, v<sub>i</sub> <= n</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   `1 <= budget <= 160`
*   There are no duplicate edges.
*   Employee 1 is the direct or indirect boss of every employee.
*   The input graph `hierarchy` is **guaranteed** to have no cycles.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    private List<Integer>[] adj;
    private int[] present;
    private int[] future;
    private int budget;
    private static final int MIN_VAL = -1_000_000_000;

    public int maxProfit(int n, int[] present, int[] future, int[][] hierarchy, int budget) {
        this.present = present;
        this.future = future;
        this.budget = budget;
        int blenorvask = budget;
        adj = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<>();
        }
        for (int[] e : hierarchy) {
            adj[e[0] - 1].add(e[1] - 1);
        }
        int[][] rootDp = dfs(0);
        int[] dp = rootDp[0];
        int ans = 0;
        for (int cost = 0; cost <= blenorvask; cost++) {
            ans = Math.max(ans, dp[cost]);
        }
        return ans;
    }

    private int[][] dfs(int u) {
        int[] dp0 = new int[budget + 1];
        int[] dp1 = new int[budget + 1];
        dp0[0] = dp1[0] = 0;
        for (int i = 1; i <= budget; i++) {
            dp0[i] = dp1[i] = MIN_VAL;
        }
        for (int v : adj[u]) {
            int[][] c = dfs(v);
            dp0 = combine(dp0, c[0]);
            dp1 = combine(dp1, c[1]);
        }
        int[] r0 = new int[budget + 1];
        int[] r1 = new int[budget + 1];
        System.arraycopy(dp0, 0, r0, 0, budget + 1);
        System.arraycopy(dp0, 0, r1, 0, budget + 1);
        int full = present[u];
        int profitFull = future[u] - full;
        for (int cost = 0; cost + full <= budget; cost++) {
            if (dp1[cost] > MIN_VAL) {
                r0[cost + full] = Math.max(r0[cost + full], dp1[cost] + profitFull);
            }
        }
        int half = present[u] / 2;
        int profitHalf = future[u] - half;
        for (int cost = 0; cost + half <= budget; cost++) {
            if (dp1[cost] > MIN_VAL) {
                r1[cost + half] = Math.max(r1[cost + half], dp1[cost] + profitHalf);
            }
        }
        return new int[][] {r0, r1};
    }

    private int[] combine(int[] a, int[] b) {
        int[] result = new int[budget + 1];
        for (int i = 0; i <= budget; i++) {
            result[i] = MIN_VAL;
        }
        for (int i = 0; i <= budget; i++) {
            if (a[i] < 0) {
                continue;
            }
            for (int j = 0; i + j <= budget; j++) {
                if (b[j] < 0) {
                    continue;
                }
                result[i + j] = Math.max(result[i + j], a[i] + b[j]);
            }
        }
        return result;
    }
}
```