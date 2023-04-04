[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2127\. Maximum Employees to Be Invited to a Meeting

Hard

A company is organizing a meeting and has a list of `n` employees, waiting to be invited. They have arranged for a large **circular** table, capable of seating **any number** of employees.

The employees are numbered from `0` to `n - 1`. Each employee has a **favorite** person and they will attend the meeting **only if** they can sit next to their favorite person at the table. The favorite person of an employee is **not** themself.

Given a **0-indexed** integer array `favorite`, where `favorite[i]` denotes the favorite person of the <code>i<sup>th</sup></code> employee, return _the **maximum number of employees** that can be invited to the meeting_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/12/14/ex1.png)

**Input:** favorite = [2,2,1,2]

**Output:** 3

**Explanation:** 

The above figure shows how the company can invite employees 0, 1, and 2, and seat them at the round table. 

All employees cannot be invited because employee 2 cannot sit beside employees 0, 1, and 3, simultaneously. 

Note that the company can also invite employees 1, 2, and 3, and give them their desired seats. 

The maximum number of employees that can be invited to the meeting is 3.

**Example 2:**

**Input:** favorite = [1,2,0]

**Output:** 3

**Explanation:** 

Each employee is the favorite person of at least one other employee, and the only way the company can invite them is if they invite every employee. 

The seating arrangement will be the same as that in the figure given in example 1: 

- Employee 0 will sit between employees 2 and 1. 

- Employee 1 will sit between employees 0 and 2. 

- Employee 2 will sit between employees 1 and 0. 
  
The maximum number of employees that can be invited to the meeting is 3.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/12/14/ex2.png)

**Input:** favorite = [3,0,1,4,1]

**Output:** 4

**Explanation:** 

The above figure shows how the company will invite employees 0, 1, 3, and 4, and seat them at the round table. 

Employee 2 cannot be invited because the two spots next to their favorite employee 1 are taken. 

So the company leaves them out of the meeting. \

The maximum number of employees that can be invited to the meeting is 4.

**Constraints:**

*   `n == favorite.length`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   `0 <= favorite[i] <= n - 1`
*   `favorite[i] != i`

## Solution

```java
import java.util.ArrayDeque;

public class Solution {
    public int maximumInvitations(int[] fav) {
        int len = fav.length;
        int[] follow = new int[len];
        for (int k : fav) {
            follow[k]++;
        }
        int[] pathlen = new int[len];
        boolean[] visit = new boolean[len];
        ArrayDeque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < len; i++) {
            if (follow[i] == 0) {
                q.offer(i);
            }
        }
        while (!q.isEmpty()) {
            int i = q.poll();
            int j = fav[i];
            visit[i] = true;
            pathlen[j] = Math.max(pathlen[j], pathlen[i] + 1);
            if (--follow[j] == 0) {
                q.offer(j);
            }
        }
        int maxCycle = 0;
        int sumPath = 0;
        int i = 0;
        while (i < len) {
            if (visit[i]) {
                i++;
            }
            int clen = 0;
            while (!visit[i]) {
                clen++;
                visit[i] = true;
                i = fav[i];
            }
            if (clen == 2) {
                sumPath += pathlen[i] + pathlen[fav[i]] + 2;
            } else {
                maxCycle = Math.max(maxCycle, clen);
            }
            i++;
        }
        return Math.max(maxCycle, sumPath);
    }
}
```