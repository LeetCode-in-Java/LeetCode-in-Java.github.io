[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2260\. Minimum Consecutive Cards to Pick Up

Medium

You are given an integer array `cards` where `cards[i]` represents the **value** of the <code>i<sup>th</sup></code> card. A pair of cards are **matching** if the cards have the **same** value.

Return _the **minimum** number of **consecutive** cards you have to pick up to have a pair of **matching** cards among the picked cards._ If it is impossible to have matching cards, return `-1`.

**Example 1:**

**Input:** cards = [3,4,2,3,4,7]

**Output:** 4

**Explanation:** We can pick up the cards [3,4,2,3] which contain a matching pair of cards with value 3.

Note that picking up the cards [4,2,3,4] is also optimal.

**Example 2:**

**Input:** cards = [1,0,5,3]

**Output:** -1

**Explanation:** There is no way to pick up a set of consecutive cards that contain a pair of matching cards. 

**Constraints:**

*   <code>1 <= cards.length <= 10<sup>5</sup></code>
*   <code>0 <= cards[i] <= 10<sup>6</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int minimumCardPickup(int[] cards) {
        int mindiff = Integer.MAX_VALUE;
        Map<Integer, Integer> map = new HashMap<>();
        int n = cards.length;
        for (int i = 0; i < n; i++) {
            if (map.containsKey(cards[i])) {
                int j = map.get(cards[i]);
                mindiff = Math.min(mindiff, i - j + 1);
            }
            map.put(cards[i], i);
        }
        if (mindiff == Integer.MAX_VALUE) {
            return -1;
        }
        return mindiff;
    }
}
```