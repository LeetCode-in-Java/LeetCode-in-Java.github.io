[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1376\. Time Needed to Inform All Employees

Medium

A company has `n` employees with a unique ID for each employee from `0` to `n - 1`. The head of the company is the one with `headID`.

Each employee has one direct manager given in the `manager` array where `manager[i]` is the direct manager of the `i-th` employee, `manager[headID] = -1`. Also, it is guaranteed that the subordination relationships have a tree structure.

The head of the company wants to inform all the company employees of an urgent piece of news. He will inform his direct subordinates, and they will inform their subordinates, and so on until all employees know about the urgent news.

The `i-th` employee needs `informTime[i]` minutes to inform all of his direct subordinates (i.e., After informTime[i] minutes, all his direct subordinates can start spreading the news).

Return _the number of minutes_ needed to inform all the employees about the urgent news.

**Example 1:**

**Input:** n = 1, headID = 0, manager = [-1], informTime = [0]

**Output:** 0

**Explanation:** The head of the company is the only employee in the company.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/02/27/graph.png)

**Input:** n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]

**Output:** 1

**Explanation:** The head of the company with id = 2 is the direct manager of all the employees in the company and needs 1 minute to inform them all.

The tree structure of the employees in the company is shown.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `0 <= headID < n`
*   `manager.length == n`
*   `0 <= manager[i] < n`
*   `manager[headID] == -1`
*   `informTime.length == n`
*   `0 <= informTime[i] <= 1000`
*   `informTime[i] == 0` if employee `i` has no subordinates.
*   It is **guaranteed** that all the employees can be informed.

## Solution

```java
@SuppressWarnings("java:S1172")
public class Solution {
    private int numMinsDFS(int index, int[] manager, int[] informTime) {
        if (manager[index] != -1) {
            informTime[index] += numMinsDFS(manager[index], manager, informTime);
            manager[index] = -1;
        }
        return informTime[index];
    }

    public int numOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        int time = informTime[headID];
        for (int i = 0; i < n; i++) {
            if (informTime[i] == 0) {
                continue;
            }
            int timei = numMinsDFS(i, manager, informTime);
            if (timei > time) {
                time = timei;
            }
        }
        return time;
    }
}
```