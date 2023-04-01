[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1185\. Day of the Week

Easy

Given a date, return the corresponding day of the week for that date.

The input is given as three integers representing the `day`, `month` and `year` respectively.

Return the answer as one of the following values `{"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}`.

**Example 1:**

**Input:** day = 31, month = 8, year = 2019

**Output:** "Saturday"

**Example 2:**

**Input:** day = 18, month = 7, year = 1999

**Output:** "Sunday"

**Example 3:**

**Input:** day = 15, month = 8, year = 1993

**Output:** "Sunday"

**Constraints:**

*   The given dates are valid dates between the years `1971` and `2100`.

## Solution

```java
public class Solution {
    public String dayOfTheWeek(int day, int month, int year) {
        int counter = 0;
        for (int i = 1971; i < year; i++) {
            if (isLeapYear(i)) {
                counter += 366;
            } else {
                counter += 365;
            }
        }
        for (int i = 1; i < month; i++) {
            counter += dayOfMonth(i);
        }
        for (int i = 1; i <= day; i++) {
            counter += 1;
        }
        if (isLeapYear(year) && month > 2) {
            counter++;
        }
        switch (counter % 7) {
            case 1:
                return "Friday";
            case 2:
                return "Saturday";
            case 3:
                return "Sunday";
            case 4:
                return "Monday";
            case 5:
                return "Tuesday";
            case 6:
                return "Wednesday";
            default:
                return "Thursday";
        }
    }

    private boolean isLeapYear(int year) {
        return ((year % 4 == 0) && (year % 100 != 0)) || year % 400 == 0;
    }

    private int dayOfMonth(int month) {
        switch (month) {
            case 1:
            case 3:
            case 5:
            case 7:
            case 8:
            case 10:
            case 12:
                return 31;
            case 4:
            case 6:
            case 9:
            case 11:
                return 30;
            default:
                return 28;
        }
    }
}
```