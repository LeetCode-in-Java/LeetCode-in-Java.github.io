[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3678\. Smallest Absent Positive Greater Than Average

Easy

You are given an integer array `nums`.

Return the **smallest absent positive** integer in `nums` such that it is **strictly greater** than the **average** of all elements in `nums`.

The **average** of an array is defined as the sum of all its elements divided by the number of elements.

**Example 1:**

**Input:** nums = [3,5]

**Output:** 6

**Explanation:**

*   The average of `nums` is `(3 + 5) / 2 = 8 / 2 = 4`.
*   The smallest absent positive integer greater than 4 is 6.

**Example 2:**

**Input:** nums = [-1,1,2]

**Output:** 3

**Explanation:**

*   The average of `nums` is `(-1 + 1 + 2) / 3 = 2 / 3 = 0.667`.
*   The smallest absent positive integer greater than 0.667 is 3.

**Example 3:**

**Input:** nums = [4,-1]

**Output:** 2

**Explanation:**

*   The average of `nums` is `(4 + (-1)) / 2 = 3 / 2 = 1.50`.
*   The smallest absent positive integer greater than 1.50 is 2.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `-100 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    public int smallestAbsent(int[] nums) {
        int sum = 0;
        for (int j : nums) {
            sum += j;
        }
        double avg = (double) sum / nums.length;
        int num;
        if (avg < 0) {
            num = 1;
        } else {
            num = (int) avg + 1;
        }
        while (true) {
            boolean flag = false;
            for (int j : nums) {
                if (num == j) {
                    flag = true;
                    break;
                }
            }
            if (!flag && num > avg) {
                return num;
            }
            num++;
        }
    }
}
```