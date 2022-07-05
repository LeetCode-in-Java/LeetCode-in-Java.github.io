[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2212\. Maximum Points in an Archery Competition

Medium

Alice and Bob are opponents in an archery competition. The competition has set the following rules:

1.  Alice first shoots `numArrows` arrows and then Bob shoots `numArrows` arrows.
2.  The points are then calculated as follows:
    1.  The target has integer scoring sections ranging from `0` to `11` **inclusive**.
    2.  For **each** section of the target with score `k` (in between `0` to `11`), say Alice and Bob have shot <code>a<sub>k</sub></code> and <code>b<sub>k</sub></code> arrows on that section respectively. If <code>a<sub>k</sub> >= b<sub>k</sub></code>, then Alice takes `k` points. If <code>a<sub>k</sub> < b<sub>k</sub></code>, then Bob takes `k` points.
    3.  However, if <code>a<sub>k</sub> == b<sub>k</sub> == 0</code>, then **nobody** takes `k` points.

*   For example, if Alice and Bob both shot `2` arrows on the section with score `11`, then Alice takes `11` points. On the other hand, if Alice shot `0` arrows on the section with score `11` and Bob shot `2` arrows on that same section, then Bob takes `11` points.


You are given the integer `numArrows` and an integer array `aliceArrows` of size `12`, which represents the number of arrows Alice shot on each scoring section from `0` to `11`. Now, Bob wants to **maximize** the total number of points he can obtain.

Return _the array_ `bobArrows` _which represents the number of arrows Bob shot on **each** scoring section from_ `0` _to_ `11`. The sum of the values in `bobArrows` should equal `numArrows`.

If there are multiple ways for Bob to earn the maximum total points, return **any** one of them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/02/24/ex1.jpg)

**Input:** numArrows = 9, aliceArrows = [1,1,0,1,0,0,2,1,0,1,2,0]

**Output:** [0,0,0,0,1,1,0,0,1,2,3,1]

**Explanation:** The table above shows how the competition is scored. Bob earns a total point of 4 + 5 + 8 + 9 + 10 + 11 = 47. It can be shown that Bob cannot obtain a score higher than 47 points.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/02/24/ex2new.jpg)

**Input:** numArrows = 3, aliceArrows = [0,0,1,0,0,0,0,0,0,0,0,2]

**Output:** [0,0,0,0,0,0,0,0,1,1,1,0]

**Explanation:** The table above shows how the competition is scored. Bob earns a total point of 8 + 9 + 10 = 27. It can be shown that Bob cannot obtain a score higher than 27 points.

**Constraints:**

*   <code>1 <= numArrows <= 10<sup>5</sup></code>
*   `aliceArrows.length == bobArrows.length == 12`
*   `0 <= aliceArrows[i], bobArrows[i] <= numArrows`
*   `sum(aliceArrows[i]) == numArrows`

## Solution

```java
public class Solution {
    private final int[] ans = new int[12];
    private final int[] ans1 = new int[12];
    private int max = 0;

    public int[] maximumBobPoints(int numArrows, int[] aliceArrows) {
        solve(numArrows, aliceArrows, 11, 0);
        return ans1;
    }

    private void solve(int numArrows, int[] aliceArrows, int index, int sum) {
        if (numArrows <= 0 || index < 0) {
            if (max < sum) {
                max = sum;
                ans1[0] = Math.max(ans[0], ans[0] + numArrows);
                System.arraycopy(ans, 1, ans1, 1, 11);
            }
            return;
        }
        if (aliceArrows[index] + 1 <= numArrows) {
            ans[index] = aliceArrows[index] + 1;
            solve(numArrows - (aliceArrows[index] + 1), aliceArrows, index - 1, sum + index);
            ans[index] = 0;
        }
        solve(numArrows, aliceArrows, index - 1, sum);
    }
}
```