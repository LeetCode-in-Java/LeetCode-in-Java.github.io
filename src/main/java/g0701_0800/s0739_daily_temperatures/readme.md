[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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

**Time Complexity (Big O Time):**

The program uses a nested loop structure to calculate the number of days until a warmer day for each temperature in the array. Here's the breakdown of the time complexity:

- The outer loop runs from `sol.length - 2` down to `0`, which iterates through each temperature in the array once. This is a linear operation, O(n), where 'n' is the length of the `temperatures` array.

- The inner while loop is used to find the next warmer day for the current temperature. In the worst case, the inner while loop can iterate through the entire remaining portion of the array (from the current position to the end). However, each element in the array is processed at most twice, as once an element is assigned a value in `sol`, it will not be revisited. Therefore, the inner while loop can be considered as an amortized O(n) operation over all iterations of the outer loop.

Therefore, the overall time complexity of the program is O(n) in the worst case, where 'n' is the length of the `temperatures` array.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the additional data structures used, including the integer array `sol`.

- The integer array `sol` is used to store the number of days until a warmer day for each temperature. It has the same length as the input array `temperatures`, so it requires O(n) space.

Therefore, the overall space complexity of the program is O(n) due to the integer array `sol`.

In summary, the provided program has a time complexity of O(n) in the worst case and a space complexity of O(n), where 'n' is the length of the `temperatures` array. This improved time complexity compared to the previous analysis is due to the fact that each element in the array is processed at most twice.
