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
        if (deck.length < 2) {
            return false;
        }
        Map<Integer, Integer> mapReps = new HashMap<>();
        for (int card : deck) {
            if (!mapReps.containsKey(card)) {
                mapReps.put(card, 1);
            } else {
                mapReps.put(card, mapReps.get(card) + 1);
            }
        }
        int num = 0;
        int[] arrReps = new int[mapReps.size()];
        for (Map.Entry<Integer, Integer> e : mapReps.entrySet()) {
            arrReps[num++] = e.getValue();
        }
        num = arrGCD(arrReps, arrReps.length);
        return num > 1;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }

    private int arrGCD(int[] arr, int n) {
        int result = arr[0];
        for (int i = 1; i < n; i++) {
            result = gcd(arr[i], result);
        }
        return result;
    }
}
```