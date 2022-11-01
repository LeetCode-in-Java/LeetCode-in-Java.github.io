[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2409\. Count Days Spent Together

Easy

Alice and Bob are traveling to Rome for separate business meetings.

You are given 4 strings `arriveAlice`, `leaveAlice`, `arriveBob`, and `leaveBob`. Alice will be in the city from the dates `arriveAlice` to `leaveAlice` (**inclusive**), while Bob will be in the city from the dates `arriveBob` to `leaveBob` (**inclusive**). Each will be a 5-character string in the format `"MM-DD"`, corresponding to the month and day of the date.

Return _the total number of days that Alice and Bob are in Rome together._

You can assume that all dates occur in the **same** calendar year, which is **not** a leap year. Note that the number of days per month can be represented as: `[31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]`.

**Example 1:**

**Input:** arriveAlice = "08-15", leaveAlice = "08-18", arriveBob = "08-16", leaveBob = "08-19"

**Output:** 3

**Explanation:** Alice will be in Rome from August 15 to August 18. Bob will be in Rome from August 16 to August 19. They are both in Rome together on August 16th, 17th, and 18th, so the answer is 3. 

**Example 2:**

**Input:** arriveAlice = "10-01", leaveAlice = "10-31", arriveBob = "11-01", leaveBob = "12-31"

**Output:** 0

**Explanation:** There is no day when Alice and Bob are in Rome together, so we return 0. 

**Constraints:**

*   All dates are provided in the format `"MM-DD"`.
*   Alice and Bob's arrival dates are **earlier than or equal to** their leaving dates.
*   The given dates are valid dates of a **non-leap** year.

## Solution

```java
public class Solution {
    private int[] dates = new int[] {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    public int countDaysTogether(
            String arriveAlice, String leaveAlice, String arriveBob, String leaveBob) {
        if (leaveAlice.compareTo(arriveBob) < 0 || leaveBob.compareTo(arriveAlice) < 0) {
            return 0;
        }
        String end = leaveAlice.compareTo(leaveBob) <= 0 ? leaveAlice : leaveBob;
        String start = arriveAlice.compareTo(arriveBob) <= 0 ? arriveBob : arriveAlice;
        String[] starts = start.split("-");
        String[] ends = end.split("-");
        Integer startMonth = Integer.valueOf(starts[0]);
        Integer endMonth = Integer.valueOf(ends[0]);
        int res = 0;
        if (startMonth.equals(endMonth)) {
            res += (Integer.valueOf(ends[1]) - Integer.valueOf(starts[1]) + 1);
            return res;
        }
        for (int i = startMonth; i <= endMonth; i++) {
            if (i == endMonth) {
                res += Integer.valueOf(ends[1]);
            } else if (i == startMonth) {
                res += dates[i] - Integer.valueOf(starts[1]) + 1;
            } else {
                res += dates[i];
            }
        }
        return res;
    }
}
```