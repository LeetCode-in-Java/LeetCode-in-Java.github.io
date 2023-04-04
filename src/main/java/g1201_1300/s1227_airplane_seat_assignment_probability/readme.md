[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1227\. Airplane Seat Assignment Probability

Medium

`n` passengers board an airplane with exactly `n` seats. The first passenger has lost the ticket and picks a seat randomly. But after that, the rest of the passengers will:

*   Take their own seat if it is still available, and
*   Pick other seats randomly when they find their seat occupied

Return _the probability that the_ <code>n<sup>th</sup></code> _person gets his own seat_.

**Example 1:**

**Input:** n = 1

**Output:** 1.00000

**Explanation:** The first person can only get the first seat.

**Example 2:**

**Input:** n = 2

**Output:** 0.50000

**Explanation:** The second person has a probability of 0.5 to get the second seat (when first person gets the first seat).

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public double nthPersonGetsNthSeat(int n) {
        if (n == 1) {
            return 1.0D;
        }
        return 0.5D;
    }
}
```