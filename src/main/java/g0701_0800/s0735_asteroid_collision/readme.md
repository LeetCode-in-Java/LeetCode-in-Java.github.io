[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 735\. Asteroid Collision

Medium

We are given an array `asteroids` of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

**Example 1:**

**Input:** asteroids = [5,10,-5]

**Output:** [5,10]

**Explanation:** The 10 and -5 collide resulting in 10. The 5 and 10 never collide.

**Example 2:**

**Input:** asteroids = [8,-8]

**Output:** []

**Explanation:** The 8 and -8 collide exploding each other.

**Example 3:**

**Input:** asteroids = [10,2,-5]

**Output:** [10]

**Explanation:** The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.

**Constraints:**

*   <code>2 <= asteroids.length <= 10<sup>4</sup></code>
*   `-1000 <= asteroids[i] <= 1000`
*   `asteroids[i] != 0`

## Solution

```java
import java.util.Deque;
import java.util.LinkedList;

public class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        Deque<Integer> stack = new LinkedList<>();
        for (int a : asteroids) {
            if (a > 0) {
                stack.addLast(a);
            } else {
                if (!stack.isEmpty() && stack.peekLast() > 0) {
                    if (stack.peekLast() == Math.abs(a)) {
                        stack.pollLast();
                    } else {
                        while (!stack.isEmpty()
                                && stack.peekLast() > 0
                                && stack.peekLast() < Math.abs(a)) {
                            stack.pollLast();
                        }
                        if (!stack.isEmpty()
                                && stack.peekLast() > 0
                                && stack.peekLast() == Math.abs(a)) {
                            stack.pollLast();
                        } else if (stack.isEmpty() || stack.peekLast() < 0) {
                            stack.addLast(a);
                        }
                    }
                } else {
                    stack.addLast(a);
                }
            }
        }
        int[] ans = new int[stack.size()];
        for (int i = stack.size() - 1; i >= 0; i--) {
            ans[i] = stack.pollLast();
        }
        return ans;
    }
}
```