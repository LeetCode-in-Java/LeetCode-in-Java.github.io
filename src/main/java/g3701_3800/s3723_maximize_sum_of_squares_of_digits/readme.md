[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3723\. Maximize Sum of Squares of Digits

Medium

You are given two **positive** integers `num` and `sum`.

A positive integer `n` is **good** if it satisfies both of the following:

*   The number of digits in `n` is **exactly** `num`.
*   The sum of digits in `n` is **exactly** `sum`.

The **score** of a **good** integer `n` is the sum of the squares of digits in `n`.

Return a **string** denoting the **good** integer `n` that achieves the **maximum** **score**. If there are multiple possible integers, return the **maximum** one. If no such integer exists, return an empty string.

**Example 1:**

**Input:** num = 2, sum = 3

**Output:** "30"

**Explanation:**

There are 3 good integers: 12, 21, and 30.

*   The score of 12 is <code>1<sup>2</sup> + 2<sup>2</sup> = 5</code>.
*   The score of 21 is <code>2<sup>2</sup> + 1<sup>2</sup> = 5</code>.
*   The score of 30 is <code>3<sup>2</sup> + 0<sup>2</sup> = 9</code>.

The maximum score is 9, which is achieved by the good integer 30. Therefore, the answer is `"30"`.

**Example 2:**

**Input:** num = 2, sum = 17

**Output:** "98"

**Explanation:**

There are 2 good integers: 89 and 98.

*   The score of 89 is <code>8<sup>2</sup> + 9<sup>2</sup> = 145</code>.
*   The score of 98 is <code>9<sup>2</sup> + 8<sup>2</sup> = 145</code>.

The maximum score is 145. The maximum good integer that achieves this score is 98. Therefore, the answer is `"98"`.

**Example 3:**

**Input:** num = 1, sum = 10

**Output:** ""

**Explanation:**

There are no integers that have exactly 1 digit and whose digits sum to 10. Therefore, the answer is `""`.

**Constraints:**

*   <code>1 <= num <= 2 * 10<sup>5</sup></code>
*   <code>1 <= sum <= 2 * 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public String maxSumOfSquares(int places, int sum) {
        String ans = "";
        int nines = sum / 9;
        if (places < nines) {
            return ans;
        } else if (places == nines) {
            int remSum = sum - nines * 9;
            if (remSum > 0) {
                return ans;
            }
            ans = "9".repeat(nines);
        } else {
            int remSum = sum - nines * 9;
            ans = "9".repeat(nines) + remSum;
            int extra = places - ans.length();
            if (extra > 0) {
                ans = ans + ("0".repeat(extra));
            }
        }
        return ans;
    }
}
```