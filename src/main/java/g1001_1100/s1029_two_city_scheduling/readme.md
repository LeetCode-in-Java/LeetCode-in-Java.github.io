[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1029\. Two City Scheduling

Medium

A company is planning to interview `2n` people. Given the array `costs` where <code>costs[i] = [aCost<sub>i</sub>, bCost<sub>i</sub>]</code>, the cost of flying the <code>i<sup>th</sup></code> person to city `a` is <code>aCost<sub>i</sub></code>, and the cost of flying the <code>i<sup>th</sup></code> person to city `b` is <code>bCost<sub>i</sub></code>.

Return _the minimum cost to fly every person to a city_ such that exactly `n` people arrive in each city.

**Example 1:**

**Input:** costs = \[\[10,20],[30,200],[400,50],[30,20]]

**Output:** 110

**Explanation:**

The first person goes to city A for a cost of 10. 

The second person goes to city A for a cost of 30. 

The third person goes to city B for a cost of 50. 

The fourth person goes to city B for a cost of 20. 

The total minimum cost is 10 + 30 + 50 + 20 = 110 to have half the people interviewing in each city.

**Example 2:**

**Input:** costs = \[\[259,770],[448,54],[926,667],[184,139],[840,118],[577,469]]

**Output:** 1859

**Example 3:**

**Input:** costs = \[\[515,563],[451,713],[537,709],[343,819],[855,779],[457,60],[650,359],[631,42]]

**Output:** 3086

**Constraints:**

*   `2 * n == costs.length`
*   `2 <= costs.length <= 100`
*   `costs.length` is even.
*   <code>1 <= aCost<sub>i</sub>, bCost<sub>i</sub> <= 1000</code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int twoCitySchedCost(int[][] costs) {
        Arrays.sort(costs, (a, b) -> (a[0] - a[1] - (b[0] - b[1])));
        int cost = 0;
        for (int i = 0; i < costs.length; i++) {
            if (i < costs.length / 2) {
                cost += costs[i][0];
            } else {
                cost += costs[i][1];
            }
        }
        return cost;
    }
}
```