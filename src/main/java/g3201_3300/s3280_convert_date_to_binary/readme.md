[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3280\. Convert Date to Binary

Easy

You are given a string `date` representing a Gregorian calendar date in the `yyyy-mm-dd` format.

`date` can be written in its binary representation obtained by converting year, month, and day to their binary representations without any leading zeroes and writing them down in `year-month-day` format.

Return the **binary** representation of `date`.

**Example 1:**

**Input:** date = "2080-02-29"

**Output:** "100000100000-10-11101"

**Explanation:**

100000100000, 10, and 11101 are the binary representations of 2080, 02, and 29 respectively.

**Example 2:**

**Input:** date = "1900-01-01"

**Output:** "11101101100-1-1"

**Explanation:**

11101101100, 1, and 1 are the binary representations of 1900, 1, and 1 respectively.

**Constraints:**

*   `date.length == 10`
*   `date[4] == date[7] == '-'`, and all other `date[i]`'s are digits.
*   The input is generated such that `date` represents a valid Gregorian calendar date between Jan 1<sup>st</sup>, 1900 and Dec 31<sup>st</sup>, 2100 (both inclusive).

## Solution

```java
public class Solution {
    private String helper(String str) {
        return Integer.toBinaryString(Integer.parseInt(str));
    }

    public String convertDateToBinary(String date) {
        StringBuilder sb = new StringBuilder();
        for (String str : date.split("-")) {
            sb.append(helper(str));
            sb.append("-");
        }
        sb.deleteCharAt(sb.length() - 1);
        return sb.toString();
    }
}
```