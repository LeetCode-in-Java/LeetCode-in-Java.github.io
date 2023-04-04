[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1344\. Angle Between Hands of a Clock

Medium

Given two numbers, `hour` and `minutes`, return _the smaller angle (in degrees) formed between the_ `hour` _and the_ `minute` _hand_.

Answers within <code>10<sup>-5</sup></code> of the actual value will be accepted as correct.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/12/26/sample_1_1673.png)

**Input:** hour = 12, minutes = 30

**Output:** 165

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/12/26/sample_2_1673.png)

**Input:** hour = 3, minutes = 30

**Output:** 75

**Example 3:**

![](https://assets.leetcode.com/uploads/2019/12/26/sample_3_1673.png)

**Input:** hour = 3, minutes = 15

**Output:** 7.5

**Constraints:**

*   `1 <= hour <= 12`
*   `0 <= minutes <= 59`

## Solution

```java
public class Solution {
    public double angleClock(int hour, int minutes) {
        double minAngle = minutes * 360.0 / 60;
        double hourAnglePart1 = hour != 12 ? (hour * 360.0) / 12 : 0;
        double hourAnglePart2 = (double) (30 * minutes) / (double) 60;
        double hourAngle = hourAnglePart1 + hourAnglePart2;
        double preResult = Math.abs(minAngle - (hourAngle));
        return preResult > 180 ? 360 - preResult : preResult;
    }
}
```