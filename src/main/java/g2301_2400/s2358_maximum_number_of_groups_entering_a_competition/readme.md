[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2358\. Maximum Number of Groups Entering a Competition

Medium

You are given a positive integer array `grades` which represents the grades of students in a university. You would like to enter **all** these students into a competition in **ordered** non-empty groups, such that the ordering meets the following conditions:

*   The sum of the grades of students in the <code>i<sup>th</sup></code> group is **less than** the sum of the grades of students in the <code>(i + 1)<sup>th</sup></code> group, for all groups (except the last).
*   The total number of students in the <code>i<sup>th</sup></code> group is **less than** the total number of students in the <code>(i + 1)<sup>th</sup></code> group, for all groups (except the last).

Return _the **maximum** number of groups that can be formed_.

**Example 1:**

**Input:** grades = [10,6,12,7,3,5]

**Output:** 3

**Explanation:** The following is a possible way to form 3 groups of students:

- 1<sup>st</sup> group has the students with grades = [12]. Sum of grades: 12. Student count: 1

- 2<sup>nd</sup> group has the students with grades = [6,7]. Sum of grades: 6 + 7 = 13. Student count: 2

- 3<sup>rd</sup> group has the students with grades = [10,3,5]. Sum of grades: 10 + 3 + 5 = 18. Student count: 3

It can be shown that it is not possible to form more than 3 groups. 

**Example 2:**

**Input:** grades = [8,8]

**Output:** 1

**Explanation:** We can only form 1 group, since forming 2 groups would lead to an equal number of students in both groups. 

**Constraints:**

*   <code>1 <= grades.length <= 10<sup>5</sup></code>
*   <code>1 <= grades[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int maximumGroups(int[] grades) {
        int len = grades.length;
        return (int) (-1 + Math.sqrt(1D + 8 * len)) / 2;
    }
}
```