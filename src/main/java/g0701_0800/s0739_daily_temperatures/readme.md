## 739\. Daily Temperatures

Medium

Given an array of integers `temperatures` represents the daily temperatures, return _an array_ `answer` _such that_ `answer[i]` _is the number of days you have to wait after the_ <code>i<sup>th</sup></code> _day to get a warmer temperature_. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

**Example 1:**

**Input:** temperatures = [73,74,75,71,69,72,76,73]

**Output:** [1,1,4,2,1,1,0,0] 

**Example 2:**

**Input:** temperatures = [30,40,50,60]

**Output:** [1,1,1,0] 

**Example 3:**

**Input:** temperatures = [30,60,90]

**Output:** [1,1,0] 

**Constraints:**

*   <code>1 <= temperatures.length <= 10<sup>5</sup></code>
*   `30 <= temperatures[i] <= 100`

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int[] sol = new int[temperatures.length];
        sol[temperatures.length - 1] = 0;
        for (int i = sol.length - 2; i >= 0; i--) {
            int j = i + 1;
            while (j <= sol.length) {
                if (temperatures[i] < temperatures[j]) {
                    sol[i] = j - i;
                    break;
                } else {
                    if (sol[j] == 0) {
                        break;
                    }
                    j = j + sol[j];
                }
            }
        }
        return sol;
    }
}
```