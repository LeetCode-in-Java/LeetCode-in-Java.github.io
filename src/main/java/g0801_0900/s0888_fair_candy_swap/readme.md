[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 888\. Fair Candy Swap

Easy

Alice and Bob have a different total number of candies. You are given two integer arrays `aliceSizes` and `bobSizes` where `aliceSizes[i]` is the number of candies of the <code>i<sup>th</sup></code> box of candy that Alice has and `bobSizes[j]` is the number of candies of the <code>j<sup>th</sup></code> box of candy that Bob has.

Since they are friends, they would like to exchange one candy box each so that after the exchange, they both have the same total amount of candy. The total amount of candy a person has is the sum of the number of candies in each box they have.

Return a_n integer array_ `answer` _where_ `answer[0]` _is the number of candies in the box that Alice must exchange, and_ `answer[1]` _is the number of candies in the box that Bob must exchange_. If there are multiple answers, you may **return any** one of them. It is guaranteed that at least one answer exists.

**Example 1:**

**Input:** aliceSizes = [1,1], bobSizes = [2,2]

**Output:** [1,2]

**Example 2:**

**Input:** aliceSizes = [1,2], bobSizes = [2,3]

**Output:** [1,2]

**Example 3:**

**Input:** aliceSizes = [2], bobSizes = [1,3]

**Output:** [2,3]

**Constraints:**

*   <code>1 <= aliceSizes.length, bobSizes.length <= 10<sup>4</sup></code>
*   <code>1 <= aliceSizes[i], bobSizes[j] <= 10<sup>5</sup></code>
*   Alice and Bob have a different total number of candies.
*   There will be at least one valid answer for the given input.

## Solution

```java
import java.util.HashSet;

public class Solution {
    public int[] fairCandySwap(int[] aliceSizes, int[] bobSizes) {
        int aSum = 0;
        int bSum = 0;
        int diff;
        int[] ans = new int[2];
        for (int bar : aliceSizes) {
            aSum += bar;
        }
        for (int bar : bobSizes) {
            bSum += bar;
        }
        diff = aSum - bSum;
        HashSet<Integer> set = new HashSet<>();
        for (int bar : aliceSizes) {
            set.add(bar);
        }
        for (int bar : bobSizes) {
            if (set.contains(bar + diff / 2)) {
                ans[0] = bar + diff / 2;
                ans[1] = bar;
                break;
            }
        }
        return ans;
    }
}
```