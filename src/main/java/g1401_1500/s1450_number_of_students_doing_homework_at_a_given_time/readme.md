[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1450\. Number of Students Doing Homework at a Given Time

Easy

Given two integer arrays `startTime` and `endTime` and given an integer `queryTime`.

The `ith` student started doing their homework at the time `startTime[i]` and finished it at time `endTime[i]`.

Return _the number of students_ doing their homework at time `queryTime`. More formally, return the number of students where `queryTime` lays in the interval `[startTime[i], endTime[i]]` inclusive.

**Example 1:**

**Input:** startTime = [1,2,3], endTime = [3,2,7], queryTime = 4

**Output:** 1

**Explanation:** We have 3 students where: 

The first student started doing homework at time 1 and finished at time 3 and wasn't doing anything at time 4. 

The second student started doing homework at time 2 and finished at time 2 and also wasn't doing anything at time 4. 

The third student started doing homework at time 3 and finished at time 7 and was the only student doing homework at time 4.

**Example 2:**

**Input:** startTime = [4], endTime = [4], queryTime = 4

**Output:** 1

**Explanation:** The only student was doing their homework at the queryTime.

**Constraints:**

*   `startTime.length == endTime.length`
*   `1 <= startTime.length <= 100`
*   `1 <= startTime[i] <= endTime[i] <= 1000`
*   `1 <= queryTime <= 1000`

## Solution

```java
public class Solution {
    public int busyStudent(int[] startTime, int[] endTime, int queryTime) {
        int count = 0;
        for (int i = 0; i < startTime.length; i++) {
            if (startTime[i] <= queryTime && endTime[i] >= queryTime) {
                count++;
            }
        }
        return count;
    }
}
```