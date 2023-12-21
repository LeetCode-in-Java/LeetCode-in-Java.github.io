[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2848\. Points That Intersect With Cars

Easy

You are given a **0-indexed** 2D integer array `nums` representing the coordinates of the cars parking on a number line. For any index `i`, <code>nums[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> where <code>start<sub>i</sub></code> is the starting point of the <code>i<sup>th</sup></code> car and <code>end<sub>i</sub></code> is the ending point of the <code>i<sup>th</sup></code> car.

Return _the number of integer points on the line that are covered with **any part** of a car._

**Example 1:**

**Input:** nums = \[\[3,6],[1,5],[4,7]]

**Output:** 7

**Explanation:** All the points from 1 to 7 intersect at least one car, therefore the answer would be 7.

**Example 2:**

**Input:** nums = \[\[1,3],[5,8]]

**Output:** 7

**Explanation:** Points intersecting at least one car are 1, 2, 3, 5, 6, 7, 8. There are a total of 7 points, therefore the answer would be 7.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `nums[i].length == 2`
*   <code>1 <= start<sub>i</sub> <= end<sub>i</sub> <= 100</code>

## Solution

```java
import java.util.List;

public class Solution {
    public int numberOfPoints(List<List<Integer>> nums) {
        int min = 101;
        int max = 0;
        int[] count = new int[102];
        for (List<Integer> list : nums) {
            int num1 = list.get(0);
            int num2 = list.get(1);

            if (num1 < min) {
                min = num1;
            }
            if (num2 > max) {
                max = num2;
            }

            count[num1]--;
            count[num2 + 1]++;
        }

        int result = 0;
        int balance = 0;
        for (; min <= max; min++) {
            balance += count[min];
            if (balance < 0) {
                result++;
            }
        }
        return result;
    }
}
```