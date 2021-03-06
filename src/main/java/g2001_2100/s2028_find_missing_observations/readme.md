[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2028\. Find Missing Observations

Medium

You have observations of `n + m` **6-sided** dice rolls with each face numbered from `1` to `6`. `n` of the observations went missing, and you only have the observations of `m` rolls. Fortunately, you have also calculated the **average value** of the `n + m` rolls.

You are given an integer array `rolls` of length `m` where `rolls[i]` is the value of the <code>i<sup>th</sup></code> observation. You are also given the two integers `mean` and `n`.

Return _an array of length_ `n` _containing the missing observations such that the **average value** of the_ `n + m` _rolls is **exactly**_ `mean`. If there are multiple valid answers, return _any of them_. If no such array exists, return _an empty array_.

The **average value** of a set of `k` numbers is the sum of the numbers divided by `k`.

Note that `mean` is an integer, so the sum of the `n + m` rolls should be divisible by `n + m`.

**Example 1:**

**Input:** rolls = [3,2,4,3], mean = 4, n = 2

**Output:** [6,6]

**Explanation:** The mean of all n + m rolls is (3 + 2 + 4 + 3 + 6 + 6) / 6 = 4.

**Example 2:**

**Input:** rolls = [1,5,6], mean = 3, n = 4

**Output:** [2,3,2,2]

**Explanation:** The mean of all n + m rolls is (1 + 5 + 6 + 2 + 3 + 2 + 2) / 7 = 3.

**Example 3:**

**Input:** rolls = [1,2,3,4], mean = 6, n = 4

**Output:** []

**Explanation:** It is impossible for the mean to be 6 no matter what the 4 missing rolls are.

**Constraints:**

*   `m == rolls.length`
*   <code>1 <= n, m <= 10<sup>5</sup></code>
*   `1 <= rolls[i], mean <= 6`

## Solution

```java
public class Solution {
    public int[] missingRolls(int[] rolls, int mean, int n) {
        int m = rolls.length;
        int msum = 0;
        int[] res = new int[n];
        for (int roll : rolls) {
            msum += roll;
        }
        int totalmn = mean * (m + n) - msum;
        if (totalmn < n || totalmn > n * 6) {
            return new int[0];
        }
        int j = 0;
        while (totalmn > 0) {
            int dice = Math.min(6, totalmn - n + 1);
            res[j] = dice;
            totalmn = totalmn - dice;
            n--;
            j++;
        }
        return res;
    }
}
```