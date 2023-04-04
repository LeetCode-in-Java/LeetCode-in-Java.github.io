[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 401\. Binary Watch

Easy

A binary watch has 4 LEDs on the top which represent the hours (0-11), and the 6 LEDs on the bottom represent the minutes (0-59). Each LED represents a zero or one, with the least significant bit on the right.

*   For example, the below binary watch reads `"4:51"`.

![](https://assets.leetcode.com/uploads/2021/04/08/binarywatch.jpg)

Given an integer `turnedOn` which represents the number of LEDs that are currently on, return _all possible times the watch could represent_. You may return the answer in **any order**.

The hour must not contain a leading zero.

*   For example, `"01:00"` is not valid. It should be `"1:00"`.

The minute must be consist of two digits and may contain a leading zero.

*   For example, `"10:2"` is not valid. It should be `"10:02"`.

**Example 1:**

**Input:** turnedOn = 1

**Output:** ["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"] 

**Example 2:**

**Input:** turnedOn = 9

**Output:** [] 

**Constraints:**

*   `0 <= turnedOn <= 10`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<String> readBinaryWatch(int turnedOn) {
        List<String> times = new ArrayList<>();
        for (int hour = 0; hour <= 11; hour++) {
            for (int minutes = 0; minutes <= 59; minutes++) {
                readBinaryWatchHelper(turnedOn, times, hour, minutes);
            }
        }
        return times;
    }

    private void readBinaryWatchHelper(
            int turnedOn, List<String> selectedTimes, int hour, int minutes) {
        if (isValidTime(turnedOn, hour, minutes)) {
            selectedTimes.add(getTimeString(hour, minutes));
        }
    }

    private String getTimeString(int hour, int minutes) {
        StringBuilder time = new StringBuilder();
        time.append(hour);
        time.append(':');
        if (minutes < 10) {
            time.append('0');
        }
        time.append(minutes);
        return time.toString();
    }

    private boolean isValidTime(int turnedOn, int hour, int minutes) {
        int counter = 0;
        while (hour != 0) {
            if ((hour & 1) == 1) {
                counter++;
            }
            hour >>>= 1;
        }
        if (counter > turnedOn) {
            return false;
        }
        while (minutes != 0) {
            if ((minutes & 1) == 1) {
                counter++;
            }
            minutes >>>= 1;
        }
        return counter == turnedOn;
    }
}
```