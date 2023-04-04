[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1696\. Jump Game VI

Medium

You are given a **0-indexed** integer array `nums` and an integer `k`.

You are initially standing at index `0`. In one move, you can jump at most `k` steps forward without going outside the boundaries of the array. That is, you can jump from index `i` to any index in the range `[i + 1, min(n - 1, i + k)]` **inclusive**.

You want to reach the last index of the array (index `n - 1`). Your **score** is the **sum** of all `nums[j]` for each index `j` you visited in the array.

Return _the **maximum score** you can get_.

**Example 1:**

**Input:** nums = [1,\-1,-2,4,-7,3], k = 2

**Output:** 7

**Explanation:** You can choose your jumps forming the subsequence [1,-1,4,3] (underlined above). The sum is 7.

**Example 2:**

**Input:** nums = [10,-5,-2,4,0,3], k = 3

**Output:** 17

**Explanation:** You can choose your jumps forming the subsequence [10,4,3] (underlined above). The sum is 17.

**Example 3:**

**Input:** nums = [1,-5,-20,4,-1,3,-6,-3], k = 2

**Output:** 0

**Constraints:**

*   <code>1 <= nums.length, k <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>

## Solution

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Solution {
    public int maxResult(int[] nums, int k) {
        Deque<int[]> deque = new ArrayDeque<>();
        deque.offer(new int[] {0, nums[0]});
        for (int i = 1; i < nums.length; i++) {
            int max = deque.peek()[1];
            int[] next = new int[] {i, max + nums[i]};
            while (!deque.isEmpty() && deque.peekLast()[1] <= next[1]) {
                // PURGE FROM THE END
                deque.pollLast();
            }
            deque.offer(next);
            if (deque.peekFirst()[0] <= i - k) {
                // PURGE FROM THE HEAD
                deque.pollFirst();
            }
        }
        return deque.peekLast()[1];
    }
}
```