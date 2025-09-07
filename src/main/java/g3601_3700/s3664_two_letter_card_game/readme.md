[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3664\. Two-Letter Card Game

Medium

You are given a deck of cards represented by a string array `cards`, and each card displays two lowercase letters.

You are also given a letter `x`. You play a game with the following rules:

*   Start with 0 points.
*   On each turn, you must find two **compatible** cards from the deck that both contain the letter `x` in any position.
*   Remove the pair of cards and earn **1 point**.
*   The game ends when you can no longer find a pair of compatible cards.

Return the **maximum** number of points you can gain with optimal play.

Two cards are **compatible** if the strings differ in **exactly** 1 position.

**Example 1:**

**Input:** cards = ["aa","ab","ba","ac"], x = "a"

**Output:** 2

**Explanation:**

*   On the first turn, select and remove cards `"ab"` and `"ac"`, which are compatible because they differ at only index 1.
*   On the second turn, select and remove cards `"aa"` and `"ba"`, which are compatible because they differ at only index 0.

Because there are no more compatible pairs, the total score is 2.

**Example 2:**

**Input:** cards = ["aa","ab","ba"], x = "a"

**Output:** 1

**Explanation:**

*   On the first turn, select and remove cards `"aa"` and `"ba"`.

Because there are no more compatible pairs, the total score is 1.

**Example 3:**

**Input:** cards = ["aa","ab","ba","ac"], x = "b"

**Output:** 0

**Explanation:**

The only cards that contain the character `'b'` are `"ab"` and `"ba"`. However, they differ in both indices, so they are not compatible. Thus, the output is 0.

**Constraints:**

*   <code>2 <= cards.length <= 10<sup>5</sup></code>
*   `cards[i].length == 2`
*   Each `cards[i]` is composed of only lowercase English letters between `'a'` and `'j'`.
*   `x` is a lowercase English letter between `'a'` and `'j'`.

## Solution

```java
public class Solution {
    public int score(String[] cards, char x) {
        // store input midway as required
        // counts for "x?" group by second char and "?x" group by first char
        int[] left = new int[10];
        int[] right = new int[10];
        int xx = 0;
        for (String c : cards) {
            char a = c.charAt(0);
            char b = c.charAt(1);
            if (a == x && b == x) {
                xx++;
            } else if (a == x) {
                left[b - 'a']++;
            } else if (b == x) {
                right[a - 'a']++;
            }
        }
        // max pairs inside a group where pairs must come from different buckets:
        // pairs = min(total/2, total - maxBucket)
        int l = 0;
        int maxL = 0;
        for (int v : left) {
            l += v;
            if (v > maxL) {
                maxL = v;
            }
        }
        int r = 0;
        int maxR = 0;
        for (int v : right) {
            r += v;
            if (v > maxR) {
                maxR = v;
            }
        }
        int pairsLeft = Math.min(l / 2, l - maxL);
        int pairsRight = Math.min(r / 2, r - maxR);
        // leftovers after internal pairing
        int leftoverL = l - 2 * pairsLeft;
        int leftoverR = r - 2 * pairsRight;
        int leftovers = leftoverL + leftoverR;
        // First, use "xx" to pair with any leftovers
        int useWithXX = Math.min(xx, leftovers);
        int xxLeft = xx - useWithXX;
        // If "xx" still remain, we can break existing internal pairs:
        // breaking 1 internal pair frees 2 cards, which can pair with 2 "xx" to gain +1 net point
        int extraByBreaking = Math.min(xxLeft / 2, pairsLeft + pairsRight);
        return pairsLeft + pairsRight + useWithXX + extraByBreaking;
    }
}
```