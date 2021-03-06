[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1823\. Find the Winner of the Circular Game

Medium

There are `n` friends that are playing a game. The friends are sitting in a circle and are numbered from `1` to `n` in **clockwise order**. More formally, moving clockwise from the <code>i<sup>th</sup></code> friend brings you to the <code>(i+1)<sup>th</sup></code> friend for `1 <= i < n`, and moving clockwise from the <code>n<sup>th</sup></code> friend brings you to the <code>1<sup>st</sup></code> friend.

The rules of the game are as follows:

1.  **Start** at the <code>1<sup>st</sup></code> friend.
2.  Count the next `k` friends in the clockwise direction **including** the friend you started at. The counting wraps around the circle and may count some friends more than once.
3.  The last friend you counted leaves the circle and loses the game.
4.  If there is still more than one friend in the circle, go back to step `2` **starting** from the friend **immediately clockwise** of the friend who just lost and repeat.
5.  Else, the last friend in the circle wins the game.

Given the number of friends, `n`, and an integer `k`, return _the winner of the game_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/25/ic234-q2-ex11.png)

**Input:** n = 5, k = 2

**Output:** 3

**Explanation:** Here are the steps of the game: 

1) Start at friend 1. 

2) Count 2 friends clockwise, which are friends 1 and 2. 

3) Friend 2 leaves the circle. Next start is friend 3. 

4) Count 2 friends clockwise, which are friends 3 and 4. 

5) Friend 4 leaves the circle. Next start is friend 5. 

6) Count 2 friends clockwise, which are friends 5 and 1. 

7) Friend 1 leaves the circle. Next start is friend 3. 

8) Count 2 friends clockwise, which are friends 3 and 5. 

9) Friend 5 leaves the circle. Only friend 3 is left, so they are the winner.

**Example 2:**

**Input:** n = 6, k = 5

**Output:** 1

**Explanation:** The friends leave in this order: 5, 4, 6, 2, 3. The winner is friend 1.

**Constraints:**

*   `1 <= k <= n <= 500`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int findTheWinner(int n, int k) {
        List<Integer> list = new ArrayList<>(n);
        for (int i = 0; i < n; i++) {
            list.add(i + 1);
        }
        int startIndex = 0;
        while (list.size() != 1) {
            int removeIndex = (startIndex + k - 1) % list.size();
            list.remove(removeIndex);
            startIndex = removeIndex;
        }
        return list.get(0);
    }
}
```