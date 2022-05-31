## 2029\. Stone Game IX

Medium

Alice and Bob continue their games with stones. There is a row of n stones, and each stone has an associated value. You are given an integer array `stones`, where `stones[i]` is the **value** of the <code>i<sup>th</sup></code> stone.

Alice and Bob take turns, with **Alice** starting first. On each turn, the player may remove any stone from `stones`. The player who removes a stone **loses** if the **sum** of the values of **all removed stones** is divisible by `3`. Bob will win automatically if there are no remaining stones (even if it is Alice's turn).

Assuming both players play **optimally**, return `true` _if Alice wins and_ `false` _if Bob wins_.

**Example 1:**

**Input:** stones = [2,1]

**Output:** true

**Explanation:** The game will be played as follows: 

- Turn 1: Alice can remove either stone. 

- Turn 2: Bob removes the remaining stone. 
  
The sum of the removed stones is 1 + 2 = 3 and is divisible by 3. Therefore, Bob loses and Alice wins the game.

**Example 2:**

**Input:** stones = [2]

**Output:** false

**Explanation:** Alice will remove the only stone, and the sum of the values on the removed stones is 2. 

Since all the stones are removed and the sum of values is not divisible by 3, Bob wins the game.

**Example 3:**

**Input:** stones = [5,1,2,4,3]

**Output:** false

**Explanation:** Bob will always win. One possible way for Bob to win is shown below: 

- Turn 1: Alice can remove the second stone with value 1. Sum of removed stones = 1. 

- Turn 2: Bob removes the fifth stone with value 3. Sum of removed stones = 1 + 3 = 4. 

- Turn 3: Alices removes the fourth stone with value 4. Sum of removed stones = 1 + 3 + 4 = 8. 

- Turn 4: Bob removes the third stone with value 2. Sum of removed stones = 1 + 3 + 4 + 2 = 10. 

- Turn 5: Alice removes the first stone with value 5. Sum of removed stones = 1 + 3 + 4 + 2 + 5 = 15.
  
Alice loses the game because the sum of the removed stones (15) is divisible by 3. Bob wins the game.

**Constraints:**

*   <code>1 <= stones.length <= 10<sup>5</sup></code>
*   <code>1 <= stones[i] <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    private int[] stones;

    public boolean stoneGameIX(int[] stones) {
        this.stones = stones;
        int[] freq = new int[3];
        for (int i : stones) {
            if (i % 3 == 0) {
                freq[0]++;
            } else if (i % 3 == 1) {
                freq[1]++;
            } else {
                freq[2]++;
            }
        }
        boolean b1 = false;
        boolean b2 = false;
        int[] a = freq.clone();
        int[] b = freq.clone();
        if (a[1] > 0) {
            a[1]--;
            b1 = fun(a, 1);
        }
        if (b[2] > 0) {
            b[2]--;
            b2 = fun(b, 2);
        }
        return b1 || b2;
    }

    private boolean fun(int[] freq, int sum) {
        int n = stones.length;
        int i = 1;
        while (i < n) {
            if (i % 2 == 0) {
                if (sum % 3 == 1) {
                    if (freq[0] > 0) {
                        freq[0]--;
                    } else if (freq[1] > 0) {
                        freq[1]--;
                        sum += 1;
                    } else {
                        return false;
                    }
                } else if (sum % 3 == 2) {
                    if (freq[0] > 0) {
                        freq[0]--;
                    } else if (freq[2] > 0) {
                        freq[2]--;
                        sum += 2;
                    } else {
                        return false;
                    }
                }
            } else {
                if (sum % 3 == 2) {
                    if (freq[0] > 0) {
                        freq[0]--;
                    } else if (freq[2] > 0) {
                        freq[2]--;
                        sum += 2;
                    } else {
                        return true;
                    }
                } else if (sum % 3 == 1) {
                    if (freq[0] > 0) {
                        freq[0]--;
                    } else if (freq[1] > 0) {
                        freq[1]--;
                        sum += 1;
                    } else {
                        return true;
                    }
                }
            }
            i++;
        }
        return false;
    }
}
```