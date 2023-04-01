[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1335\. Minimum Difficulty of a Job Schedule

Hard

You want to schedule a list of jobs in `d` days. Jobs are dependent (i.e To work on the <code>i<sup>th</sup></code> job, you have to finish all the jobs `j` where `0 <= j < i`).

You have to finish **at least** one task every day. The difficulty of a job schedule is the sum of difficulties of each day of the `d` days. The difficulty of a day is the maximum difficulty of a job done on that day.

You are given an integer array `jobDifficulty` and an integer `d`. The difficulty of the <code>i<sup>th</sup></code> job is `jobDifficulty[i]`.

Return _the minimum difficulty of a job schedule_. If you cannot find a schedule for the jobs return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/16/untitled.png)

**Input:** jobDifficulty = [6,5,4,3,2,1], d = 2

**Output:** 7

**Explanation:** First day you can finish the first 5 jobs, total difficulty = 6. 

Second day you can finish the last job, total difficulty = 1.

The difficulty of the schedule = 6 + 1 = 7

**Example 2:**

**Input:** jobDifficulty = [9,9,9], d = 4

**Output:** -1

**Explanation:** If you finish a job per day you will still have a free day. you cannot find a schedule for the given jobs.

**Example 3:**

**Input:** jobDifficulty = [1,1,1], d = 3

**Output:** 3

**Explanation:** The schedule is one job per day. total difficulty will be 3.

**Constraints:**

*   `1 <= jobDifficulty.length <= 300`
*   `0 <= jobDifficulty[i] <= 1000`
*   `1 <= d <= 10`

## Solution

```java
public class Solution {
    public int minDifficulty(int[] jobDifficulty, int d) {
        int totalJobs = jobDifficulty.length;
        if (totalJobs < d) {
            return -1;
        }
        int maxJobsOneDay = totalJobs - d + 1;
        int[] map = new int[totalJobs];
        int maxDiff = Integer.MIN_VALUE;
        for (int k = totalJobs - 1; k > totalJobs - 1 - maxJobsOneDay; k--) {
            maxDiff = Math.max(maxDiff, jobDifficulty[k]);
            map[k] = maxDiff;
        }
        for (int day = d - 1; day > 0; day--) {
            int maxEndIndex = (totalJobs - 1) - (d - day);
            int maxStartIndex = maxEndIndex - maxJobsOneDay + 1;
            for (int startIndex = maxStartIndex; startIndex <= maxEndIndex; startIndex++) {
                map[startIndex] = Integer.MAX_VALUE;
                int maxDiffOfTheDay = Integer.MIN_VALUE;
                for (int endIndex = startIndex; endIndex <= maxEndIndex; endIndex++) {
                    maxDiffOfTheDay = Math.max(maxDiffOfTheDay, jobDifficulty[endIndex]);
                    int totalDiff = maxDiffOfTheDay + map[endIndex + 1];
                    map[startIndex] = Math.min(map[startIndex], totalDiff);
                }
            }
        }
        return map[0];
    }
}
```