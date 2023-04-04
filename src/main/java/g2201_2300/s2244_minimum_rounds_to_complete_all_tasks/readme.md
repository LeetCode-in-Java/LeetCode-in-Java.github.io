[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2244\. Minimum Rounds to Complete All Tasks

Medium

You are given a **0-indexed** integer array `tasks`, where `tasks[i]` represents the difficulty level of a task. In each round, you can complete either 2 or 3 tasks of the **same difficulty level**.

Return _the **minimum** rounds required to complete all the tasks, or_ `-1` _if it is not possible to complete all the tasks._

**Example 1:**

**Input:** tasks = [2,2,3,3,2,4,4,4,4,4]

**Output:** 4

**Explanation:** To complete all the tasks, a possible plan is:

- In the first round, you complete 3 tasks of difficulty level 2.

- In the second round, you complete 2 tasks of difficulty level 3.

- In the third round, you complete 3 tasks of difficulty level 4.

- In the fourth round, you complete 2 tasks of difficulty level 4.

It can be shown that all the tasks cannot be completed in fewer than 4 rounds, so the answer is 4. 

**Example 2:**

**Input:** tasks = [2,3,3]

**Output:** -1

**Explanation:** There is only 1 task of difficulty level 2, but in each round, you can only complete either 2 or 3 tasks of the same difficulty level. Hence, you cannot complete all the tasks, and the answer is -1. 

**Constraints:**

*   <code>1 <= tasks.length <= 10<sup>5</sup></code>
*   <code>1 <= tasks[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minimumRounds(int[] tasks) {
        Arrays.sort(tasks);
        int rounds = 0;
        int c = 1;
        for (int i = 0; i < tasks.length - 1; i++) {
            if (tasks[i] == tasks[i + 1]) {
                c++;
            } else {
                if (c == 1) {
                    return -1;
                }
                if (c % 3 == 0) {
                    rounds += c / 3;
                }
                if (c % 3 >= 1) {
                    rounds += c / 3 + 1;
                }
                c = 1;
            }
        }
        if (c == 1) {
            return -1;
        }
        if (c % 3 == 0) {
            rounds += c / 3;
        }
        if (c % 3 >= 1) {
            rounds += c / 3 + 1;
        }
        return rounds;
    }
}
```