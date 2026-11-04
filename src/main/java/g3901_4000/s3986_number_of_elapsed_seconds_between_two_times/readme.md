[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3986\. Number of Elapsed Seconds Between Two Times

Easy

You are given two valid times `startTime` and `endTime`, each represented as a string in the format `"HH:MM:SS"`.

Return the number of seconds that have elapsed from `startTime` to `endTime`.

**Example 1:**

**Input:** startTime = "01:00:00", endTime = "01:00:25"

**Output:** 25

**Explanation:**

`endTime` is 25 seconds ahead of `startTime`.

**Example 2:**

**Input:** startTime = "12:34:56", endTime = "13:00:00"

**Output:** 1504

**Explanation:**

`endTime` is 25 minutes and 4 seconds ahead of `startTime`, which equals 1504 seconds.

**Constraints:**

*   `startTime.length == 8`
*   `endTime.length == 8`
*   `startTime` and `endTime` are valid times in the format `"HH:MM:SS"`
*   `00 <= HH <= 23`
*   `00 <= MM <= 59`
*   `00 <= SS <= 59`
*   `endTime` is not earlier than `startTime`

## Solution

```java
public class Solution {
    public int secondsBetweenTimes(String startTime, String endTime) {
        int startSec = 0;
        int endSec = 0;
        int sSec = 0;
        int eSec = 0;
        int colon = 2;
        for (int i = 0; i < 8; i++) {
            if (startTime.charAt(i) == ':') {
                startSec = (int) (startSec + sSec * Math.pow(60, colon));
                endSec = (int) (endSec + eSec * Math.pow(60, colon));
                colon--;
                sSec = 0;
                eSec = 0;
                continue;
            }
            sSec = sSec * 10 + (startTime.charAt(i) - '0');
            eSec = eSec * 10 + (endTime.charAt(i) - '0');
        }
        startSec = (int) (startSec + sSec * Math.pow(60, colon));
        endSec = (int) (endSec + eSec * Math.pow(60, colon));
        return endSec - startSec;
    }
}
```