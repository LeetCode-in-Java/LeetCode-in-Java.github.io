[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 949\. Largest Time for Given Digits

Medium

Given an array `arr` of 4 digits, find the latest 24-hour time that can be made using each digit **exactly once**.

24-hour times are formatted as `"HH:MM"`, where `HH` is between `00` and `23`, and `MM` is between `00` and `59`. The earliest 24-hour time is `00:00`, and the latest is `23:59`.

Return _the latest 24-hour time in `"HH:MM"` format_. If no valid time can be made, return an empty string.

**Example 1:**

**Input:** arr = [1,2,3,4]

**Output:** "23:41"

**Explanation:** The valid 24-hour times are "12:34", "12:43", "13:24", "13:42", "14:23", "14:32", "21:34", "21:43", "23:14", and "23:41". Of these times, "23:41" is the latest.

**Example 2:**

**Input:** arr = [5,5,5,5]

**Output:** ""

**Explanation:** There are no valid 24-hour times as "55:55" is not valid.

**Constraints:**

*   `arr.length == 4`
*   `0 <= arr[i] <= 9`

## Solution

```java
public class Solution {
    public String largestTimeFromDigits(int[] arr) {
        StringBuilder buf = new StringBuilder();
        String maxHour = "";
        for (int i = 0; i < 4; i++) {
            for (int j = 0; j < 4; j++) {
                if (i != j) {
                    String hour = getTime(arr, i, j, 23, false);
                    String min = getTime(arr, i, j, 59, true);
                    if (hour != null && min != null && hour.compareTo(maxHour) > 0) {
                        buf.setLength(0);
                        buf.append(hour).append(':').append(min);
                        maxHour = hour;
                    }
                }
            }
        }
        return buf.toString();
    }

    private String getTime(int[] arr, int i, int j, int limit, boolean exclude) {
        int n1 = -1;
        int n2 = -1;
        for (int k = 0; k < 4; k++) {
            if ((exclude && k != i && k != j) || (!exclude && (k == i || k == j))) {
                if (n1 == -1) {
                    n1 = arr[k];
                } else {
                    n2 = arr[k];
                }
            }
        }
        String s1 = String.valueOf(n1) + n2;
        String s2 = String.valueOf(n2) + n1;
        int v1 = Integer.parseInt(s1);
        if (v1 > limit) {
            v1 = -1;
            s1 = null;
        }
        int v2 = Integer.parseInt(s2);
        if (v2 > limit) {
            v2 = -1;
            s2 = null;
        }
        return v1 >= v2 ? s1 : s2;
    }
}
```