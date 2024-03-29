[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2347\. Best Poker Hand

Easy

You are given an integer array `ranks` and a character array `suits`. You have `5` cards where the <code>i<sup>th</sup></code> card has a rank of `ranks[i]` and a suit of `suits[i]`.

The following are the types of **poker hands** you can make from best to worst:

1.  `"Flush"`: Five cards of the same suit.
2.  `"Three of a Kind"`: Three cards of the same rank.
3.  `"Pair"`: Two cards of the same rank.
4.  `"High Card"`: Any single card.

Return _a string representing the **best** type of **poker hand** you can make with the given cards._

**Note** that the return values are **case-sensitive**.

**Example 1:**

**Input:** ranks = [13,2,3,1,9], suits = ["a","a","a","a","a"]

**Output:** "Flush"

**Explanation:** The hand with all the cards consists of 5 cards with the same suit, so we have a "Flush". 

**Example 2:**

**Input:** ranks = [4,4,2,4,4], suits = ["d","a","a","b","c"]

**Output:** "Three of a Kind"

**Explanation:** The hand with the first, second, and fourth card consists of 3 cards with the same rank, so we have a "Three of a Kind".

Note that we could also make a "Pair" hand but "Three of a Kind" is a better hand.

Also note that other cards could be used to make the "Three of a Kind" hand.

**Example 3:**

**Input:** ranks = [10,10,2,12,9], suits = ["a","b","c","a","d"]

**Output:** "Pair"

**Explanation:** The hand with the first and second card consists of 2 cards with the same rank, so we have a "Pair".

Note that we cannot make a "Flush" or a "Three of a Kind". 

**Constraints:**

*   `ranks.length == suits.length == 5`
*   `1 <= ranks[i] <= 13`
*   `'a' <= suits[i] <= 'd'`
*   No two cards have the same rank and suit.

## Solution

```java
import java.util.HashMap;

public class Solution {
    public String bestHand(int[] ranks, char[] suits) {
        HashMap<Character, Integer> map = new HashMap<>();
        for (char suit : suits) {
            if (map.containsKey(suit)) {
                map.put(suit, map.get(suit) + 1);
                if (map.get(suit) == 5) {
                    return "Flush";
                }
            } else {
                map.put(suit, 1);
            }
        }
        String s = "";
        HashMap<Integer, Integer> map2 = new HashMap<>();
        for (int rank : ranks) {
            if (map2.containsKey(rank)) {
                map2.put(rank, map2.get(rank) + 1);
                if (map2.get(rank) == 2) {
                    s = "Pair";
                } else if (map2.get(rank) == 3) {
                    s = "Three of a Kind";
                    return s;
                }
            } else {
                map2.put(rank, 1);
            }
        }
        return s.isEmpty() ? "High Card" : s;
    }
}
```