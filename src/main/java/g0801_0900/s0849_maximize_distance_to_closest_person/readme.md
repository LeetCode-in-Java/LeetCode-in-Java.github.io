[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 849\. Maximize Distance to Closest Person

Medium

You are given an array representing a row of `seats` where `seats[i] = 1` represents a person sitting in the <code>i<sup>th</sup></code> seat, and `seats[i] = 0` represents that the <code>i<sup>th</sup></code> seat is empty **(0-indexed)**.

There is at least one empty seat, and at least one person sitting.

Alex wants to sit in the seat such that the distance between him and the closest person to him is maximized.

Return _that maximum distance to the closest person_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/10/distance.jpg)

**Input:** seats = [1,0,0,0,1,0,1]

**Output:** 2

**Explanation:**

If Alex sits in the second open seat (i.e. seats[2]), then the closest person has distance 2. 

If Alex sits in any other open seat, the closest person has distance 1. 

Thus, the maximum distance to the closest person is 2.

**Example 2:**

**Input:** seats = [1,0,0,0]

**Output:** 3

**Explanation:**

If Alex sits in the last seat (i.e. seats[3]), the closest person is 3 seats away. 

This is the maximum distance possible, so the answer is 3.

**Example 3:**

**Input:** seats = [0,1]

**Output:** 1

**Constraints:**

*   <code>2 <= seats.length <= 2 * 10<sup>4</sup></code>
*   `seats[i]` is `0` or `1`.
*   At least one seat is **empty**.
*   At least one seat is **occupied**.

## Solution

```java
public class Solution {
    private int maxDist = 0;

    public int maxDistToClosest(int[] seats) {
        for (int i = 0; i < seats.length; i++) {
            if (seats[i] == 0) {
                extend(seats, i);
            }
        }
        return maxDist;
    }

    private void extend(int[] seats, int position) {
        int left = position - 1;
        int right = position + 1;
        int leftMinDistance = 1;
        while (left >= 0) {
            if (seats[left] == 0) {
                leftMinDistance++;
                left--;
            } else {
                break;
            }
        }
        int rightMinDistance = 1;
        while (right < seats.length) {
            if (seats[right] == 0) {
                rightMinDistance++;
                right++;
            } else {
                break;
            }
        }
        int maxReach;
        if (position == 0) {
            maxReach = rightMinDistance;
        } else if (position == seats.length - 1) {
            maxReach = leftMinDistance;
        } else {
            maxReach = Math.min(leftMinDistance, rightMinDistance);
        }
        maxDist = Math.max(maxDist, maxReach);
    }
}
```