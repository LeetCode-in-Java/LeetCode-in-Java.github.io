[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2320\. Count Number of Ways to Place Houses

Medium

There is a street with `n * 2` **plots**, where there are `n` plots on each side of the street. The plots on each side are numbered from `1` to `n`. On each plot, a house can be placed.

Return _the number of ways houses can be placed such that no two houses are adjacent to each other on the same side of the street_. Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

Note that if a house is placed on the <code>i<sup>th</sup></code> plot on one side of the street, a house can also be placed on the <code>i<sup>th</sup></code> plot on the other side of the street.

**Example 1:**

**Input:** n = 1

**Output:** 4

**Explanation:**

Possible arrangements:

1. All plots are empty.

2. A house is placed on one side of the street.

3. A house is placed on the other side of the street.

4. Two houses are placed, one on each side of the street.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/05/12/arrangements.png)

**Input:** n = 2

**Output:** 9

**Explanation:** The 9 possible arrangements are shown in the diagram above.

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int countHousePlacements(int n) {
        // algo - 1st solve one side  of the street
        // think 0 - space , 1 - house
        // if n = 1 then we can take one 0 and one 1 (total ways = 2)
        // if n = 2 then 00 , 01 , 10 , 11 but we cant take 11 as two house cant be adjacent.
        // so the 1 ended string will be only 1 which is same as previous 0 ended string and 0 ended
        // string are 2 which is previous sum(total ways)
        // apply this formula for n no's
        long mod = 1000000007;
        long space = 1;
        long house = 1;
        long sum = space + house;
        while (--n > 0) {
            house = space;
            space = sum;
            sum = (house + space) % mod;
        }
        // as street has two side
        return (int) ((sum * sum) % mod);
    }
}
```