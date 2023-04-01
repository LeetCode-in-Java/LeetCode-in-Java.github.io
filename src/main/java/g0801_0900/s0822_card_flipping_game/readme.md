[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 822\. Card Flipping Game

Medium

You are given `n` cards, with a positive integer printed on the front and back of each card (possibly different). You can flip any number of cards (possibly zero).

After choosing the front and the back of each card, you will pick each card, and if the integer printed on the back of this card is not printed on the front of any other card, then this integer is **good**.

You are given two integer array `fronts` and `backs` where `fronts[i]` and `backs[i]` are the integers printer on the front and the back of the <code>i<sup>th</sup></code> card respectively.

Return _the smallest good and integer after flipping the cards_. If there are no good integers, return `0`.

**Note** that a **flip** swaps the front and back numbers, so the value on the front is now on the back and vice versa.

**Example 1:**

**Input:** fronts = [1,2,4,4,7], backs = [1,3,4,1,3]

**Output:** 2

**Explanation:** 

If we flip the second card, the fronts are [1,3,4,4,7] and the backs are [1,2,4,1,3]. 

We choose the second card, which has the number 2 on the back, and it is not on the front of any card, so 2 is good.

**Example 2:**

**Input:** fronts = [1], backs = [1]

**Output:** 0

**Constraints:**

*   `n == fronts.length`
*   `n == backs.length`
*   `1 <= n <= 1000`
*   `1 <= fronts[i], backs[i] <= 2000`

## Solution

```java
public class Solution {
    public int flipgame(int[] fronts, int[] backs) {
        int max = findMax(fronts, backs);
        int value = 10000;
        int[] twinCardHash = new int[max + 1];
        int[] existingNumbersHash = new int[max + 1];
        for (int i = 0; i < fronts.length; i++) {
            if (fronts[i] == backs[i]) {
                twinCardHash[fronts[i]]++;
            }
            existingNumbersHash[fronts[i]]++;
            existingNumbersHash[backs[i]]++;
        }
        for (int i = 1; i <= max; i++) {
            if ((twinCardHash[i] == 0) && (i < value) && (existingNumbersHash[i] != 0)) {
                value = i;
                break;
            }
        }
        if (value == 10000) {
            return 0;
        } else {
            return value;
        }
    }

    private static int findMax(int[] fronts, int[] backs) {
        int max = 0;
        for (int front : fronts) {
            if (max < front) {
                max = front;
            }
        }
        for (int back : backs) {
            if (max < back) {
                max = back;
            }
        }
        return max;
    }
}
```