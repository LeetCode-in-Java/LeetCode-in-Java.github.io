[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1986\. Minimum Number of Work Sessions to Finish the Tasks

Medium

There are `n` tasks assigned to you. The task times are represented as an integer array `tasks` of length `n`, where the <code>i<sup>th</sup></code> task takes `tasks[i]` hours to finish. A **work session** is when you work for **at most** `sessionTime` consecutive hours and then take a break.

You should finish the given tasks in a way that satisfies the following conditions:

*   If you start a task in a work session, you must complete it in the **same** work session.
*   You can start a new task **immediately** after finishing the previous one.
*   You may complete the tasks in **any order**.

Given `tasks` and `sessionTime`, return _the **minimum** number of **work sessions** needed to finish all the tasks following the conditions above._

The tests are generated such that `sessionTime` is **greater** than or **equal** to the **maximum** element in `tasks[i]`.

**Example 1:**

**Input:** tasks = [1,2,3], sessionTime = 3

**Output:** 2

**Explanation:** You can finish the tasks in two work sessions.

- First work session: finish the first and the second tasks in 1 + 2 = 3 hours.

- Second work session: finish the third task in 3 hours. 

**Example 2:**

**Input:** tasks = [3,1,3,1,1], sessionTime = 8

**Output:** 2

**Explanation:** You can finish the tasks in two work sessions.

- First work session: finish all the tasks except the last one in 3 + 1 + 3 + 1 = 8 hours.

- Second work session: finish the last task in 1 hour. 

**Example 3:**

**Input:** tasks = [1,2,3,4,5], sessionTime = 15

**Output:** 1

**Explanation:** You can finish all the tasks in one work session. 

**Constraints:**

*   `n == tasks.length`
*   `1 <= n <= 14`
*   `1 <= tasks[i] <= 10`
*   `max(tasks[i]) <= sessionTime <= 15`

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int minSessions(int[] tasks, int sessionTime) {
        int len = tasks.length;
        // minimum, all tasks can fit into 1 session
        int i = 1;
        // maximum, each task take 1 session to finish
        int j = len;
        while (i < j) {
            // try m sessions to see whether it can work
            int m = (i + j) / 2;
            if (canFit(tasks, new int[m], sessionTime, len - 1)) {
                j = m;
            } else {
                i = m + 1;
            }
        }
        return i;
    }

    private boolean canFit(int[] tasks, int[] sessions, int sessionTime, int idx) {
        // all tasks have been taken care of
        if (idx == -1) {
            return true;
        }
        Set<Integer> dup = new HashSet<>();
        // now to take care of tasks[idx]
        // try each spot
        for (int i = 0; i < sessions.length; i++) {
            // current spot cannot fit
            if (sessions[i] + tasks[idx] > sessionTime || dup.contains(sessions[i] + tasks[idx])) {
                continue;
            }
            dup.add(sessions[i] + tasks[idx]);
            sessions[i] += tasks[idx];
            if (canFit(tasks, sessions, sessionTime, idx - 1)) {
                return true;
            }
            sessions[i] -= tasks[idx];
        }
        return false;
    }
}
```