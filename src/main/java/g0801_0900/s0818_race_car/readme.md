[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 818\. Race Car

Hard

Your car starts at position `0` and speed `+1` on an infinite number line. Your car can go into negative positions. Your car drives automatically according to a sequence of instructions `'A'` (accelerate) and `'R'` (reverse):

*   When you get an instruction `'A'`, your car does the following:
    *   `position += speed`
    *   `speed *= 2`
*   When you get an instruction `'R'`, your car does the following:
    *   If your speed is positive then `speed = -1`
    *   otherwise `speed = 1`Your position stays the same.

For example, after commands `"AAR"`, your car goes to positions `0 --> 1 --> 3 --> 3`, and your speed goes to `1 --> 2 --> 4 --> -1`.

Given a target position `target`, return _the length of the shortest sequence of instructions to get there_.

**Example 1:**

**Input:** target = 3

**Output:** 2

**Explanation:** 

The shortest instruction sequence is "AA". 

Your position goes from 0 --> 1 --> 3.

**Example 2:**

**Input:** target = 6

**Output:** 5

**Explanation:** 

The shortest instruction sequence is "AAARA". 

Your position goes from 0 --> 1 --> 3 --> 7 --> 7 --> 6.

**Constraints:**

*   <code>1 <= target <= 10<sup>4</sup></code>

## Solution

```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int racecar(int target) {
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[] {0, 1, 0});
        while (!queue.isEmpty()) {
            int[] arr = queue.poll();
            if (arr[0] == target) {
                return arr[2];
            }
            queue.add(new int[] {arr[0] + arr[1], 2 * arr[1], 1 + arr[2]});
            if ((arr[0] + arr[1]) > target && arr[1] > 0) {
                queue.add(new int[] {arr[0], -1, 1 + arr[2]});
            }
            if ((arr[0]) + arr[1] < target && (arr[1] < 0)) {
                queue.add(new int[] {arr[0], 1, 1 + arr[2]});
            }
        }
        return -1;
    }
}
```