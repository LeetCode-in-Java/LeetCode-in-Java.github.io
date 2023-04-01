[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 914\. X of a Kind in a Deck of Cards

Easy

In a deck of cards, each card has an integer written on it.

Return `true` if and only if you can choose `X >= 2` such that it is possible to split the entire deck into 1 or more groups of cards, where:

*   Each group has exactly `X` cards.
*   All the cards in each group have the same integer.

**Example 1:**

**Input:** deck = [1,2,3,4,4,3,2,1]

**Output:** true

**Explanation:** Possible partition [1,1],[2,2],[3,3],[4,4].

**Example 2:**

**Input:** deck = [1,1,1,2,2,2,3,3]

**Output:** false

**Explanation:** No possible partition.

**Constraints:**

*   <code>1 <= deck.length <= 10<sup>4</sup></code>
*   <code>0 <= deck[i] < 10<sup>4</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public boolean hasGroupsSizeX(int[] deck) {
        HashMap<Integer, Integer> hmap = new HashMap<>();
        for (int j : deck) {
            if (hmap.containsKey(j)) {
                hmap.put(j, hmap.get(j) + 1);
            } else {
                hmap.put(j, 1);
            }
        }
        int x = hmap.get(deck[0]);
        for (Map.Entry<Integer, Integer> entry : hmap.entrySet()) {
            x = gcd(x, entry.getValue());
        }
        return x >= 2;
    }

    private int gcd(int a, int b) {
        if (b == 0) {
            return a;
        }
        return gcd(b, a % b);
    }
}
```