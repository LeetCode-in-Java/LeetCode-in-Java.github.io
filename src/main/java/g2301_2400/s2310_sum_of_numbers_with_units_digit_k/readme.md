[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2310\. Sum of Numbers With Units Digit K

Medium

Given two integers `num` and `k`, consider a set of positive integers with the following properties:

*   The units digit of each integer is `k`.
*   The sum of the integers is `num`.

Return _the **minimum** possible size of such a set, or_ `-1` _if no such set exists._

Note:

*   The set can contain multiple instances of the same integer, and the sum of an empty set is considered `0`.
*   The **units digit** of a number is the rightmost digit of the number.

**Example 1:**

**Input:** num = 58, k = 9

**Output:** 2

**Explanation:**

One valid set is [9,49], as the sum is 58 and each integer has a units digit of 9.

Another valid set is [19,39].

It can be shown that 2 is the minimum possible size of a valid set. 

**Example 2:**

**Input:** num = 37, k = 2

**Output:** -1

**Explanation:** It is not possible to obtain a sum of 37 using only integers that have a units digit of 2. 

**Example 3:**

**Input:** num = 0, k = 7

**Output:** 0

**Explanation:** The sum of an empty set is considered 0. 

**Constraints:**

*   `0 <= num <= 3000`
*   `0 <= k <= 9`

## Solution

```java
public class Solution {
    public int minimumNumbers(int nums, int k) {
        // Base Case Check
        if (nums == 0) {
            return 0;
        }
        int x = nums % 10;
        for (int i = 1; i <= 10; i++) {
            // check if the unit digits are equal for any case and if n>k*i
            if ((k * i) % 10 == x && nums >= k * i) {
                return i;
            }
        }
        // in case nothing matches
        return -1;
    }
}
```