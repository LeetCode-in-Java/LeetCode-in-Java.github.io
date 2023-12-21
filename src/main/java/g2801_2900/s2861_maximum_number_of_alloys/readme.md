[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2861\. Maximum Number of Alloys

Medium

You are the owner of a company that creates alloys using various types of metals. There are `n` different types of metals available, and you have access to `k` machines that can be used to create alloys. Each machine requires a specific amount of each metal type to create an alloy.

For the <code>i<sup>th</sup></code> machine to create an alloy, it needs `composition[i][j]` units of metal of type `j`. Initially, you have `stock[i]` units of metal type `i`, and purchasing one unit of metal type `i` costs `cost[i]` coins.

Given integers `n`, `k`, `budget`, a **1-indexed** 2D array `composition`, and **1-indexed** arrays `stock` and `cost`, your goal is to **maximize** the number of alloys the company can create while staying within the budget of `budget` coins.

**All alloys must be created with the same machine.**

Return _the maximum number of alloys that the company can create_.

**Example 1:**

**Input:** n = 3, k = 2, budget = 15, composition = \[\[1,1,1],[1,1,10]], stock = [0,0,0], cost = [1,2,3]

**Output:** 2

**Explanation:** It is optimal to use the 1<sup>st</sup> machine to create alloys. 

To create 2 alloys we need to buy the: 
- 2 units of metal of the 1<sup>st</sup> type. 
- 2 units of metal of the 2<sup>nd</sup> type. 
- 2 units of metal of the 3<sup>rd</sup> type. 

In total, we need 2 \* 1 + 2 \* 2 + 2 \* 3 = 12 coins, which is smaller than or equal to budget = 15. Notice that we have 0 units of metal of each type and we have to buy all the required units of metal. It can be proven that we can create at most 2 alloys.

**Example 2:**

**Input:** n = 3, k = 2, budget = 15, composition = \[\[1,1,1],[1,1,10]], stock = [0,0,100], cost = [1,2,3]

**Output:** 5

**Explanation:** It is optimal to use the 2<sup>nd</sup> machine to create alloys. To create 5 alloys we need to buy: 
- 5 units of metal of the 1<sup>st</sup> type. 
- 5 units of metal of the 2<sup>nd</sup> type. 
- 0 units of metal of the 3<sup>rd</sup> type. 

In total, we need 5 \* 1 + 5 \* 2 + 0 \* 3 = 15 coins, which is smaller than or equal to budget = 15. It can be proven that we can create at most 5 alloys.

**Example 3:**

**Input:** n = 2, k = 3, budget = 10, composition = \[\[2,1],[1,2],[1,1]], stock = [1,1], cost = [5,5]

**Output:** 2

**Explanation:** It is optimal to use the 3<sup>rd</sup> machine to create alloys. To create 2 alloys we need to buy the: 
- 1 unit of metal of the 1<sup>st</sup> type. 
- 1 unit of metal of the 2<sup>nd</sup> type. 

In total, we need 1 \* 5 + 1 \* 5 = 10 coins, which is smaller than or equal to budget = 10. It can be proven that we can create at most 2 alloys.

**Constraints:**

*   `1 <= n, k <= 100`
*   <code>0 <= budget <= 10<sup>8</sup></code>
*   `composition.length == k`
*   `composition[i].length == n`
*   `1 <= composition[i][j] <= 100`
*   `stock.length == cost.length == n`
*   <code>0 <= stock[i] <= 10<sup>8</sup></code>
*   `1 <= cost[i] <= 100`

## Solution

```java
import java.util.List;

public class Solution {
    public int maxNumberOfAlloys(
            int n,
            int k,
            int budget,
            List<List<Integer>> composition,
            List<Integer> stock,
            List<Integer> cost) {
        int ans = 0;
        int max = 0;
        for (int i = 0; i < n; i++) {
            max = Math.max(stock.get(i), max);
        }
        for (int i = 0; i < k; i++) {
            int temp = 0;
            int low = 0;
            int high = max + budget;
            int mid;
            while (low <= high) {
                mid = low + (high - low) / 2;
                if (isPos(i, mid, n, budget, composition, stock, cost)) {
                    low = mid + 1;
                    temp = mid;
                } else {
                    high = mid - 1;
                }
            }
            ans = Math.max(ans, temp);
        }
        return ans;
    }

    private boolean isPos(
            int idx,
            int mid,
            int n,
            int budget,
            List<List<Integer>> composition,
            List<Integer> stock,
            List<Integer> cost) {
        long paiSa = 0L;
        for (int i = 0; i < n; i++) {
            long require = ((long) composition.get(idx).get(i)) * (mid);
            long have = stock.get(i);
            long diff = require - have;
            if (diff > 0) {
                paiSa += diff * ((long) cost.get(i));
            }
            if (budget < paiSa) {
                return false;
            }
        }
        return budget >= paiSa;
    }
}
```