[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3005\. Count Elements With Maximum Frequency

Easy

You are given an array `nums` consisting of **positive** integers.

Return _the **total frequencies** of elements in_ `nums` _such that those elements all have the **maximum** frequency_.

The **frequency** of an element is the number of occurrences of that element in the array.

**Example 1:**

**Input:** nums = [1,2,2,3,1,4]

**Output:** 4

**Explanation:** The elements 1 and 2 have a frequency of 2 which is the maximum frequency in the array. So the number of elements in the array with maximum frequency is 4.

**Example 2:**

**Input:** nums = [1,2,3,4,5]

**Output:** 5

**Explanation:** All elements of the array have a frequency of 1 which is the maximum. So the number of elements in the array with maximum frequency is 5.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("java:S135")
public class Solution {
    public int maxFrequencyElements(int[] nums) {
        if (nums.length == 1) {
            return 1;
        }
        List<Integer> list = new ArrayList<>();
        int co = 0;
        int prev = 0;
        for (int num : nums) {
            if (list.contains(num)) {
                continue;
            }
            list.add(num);
            if (list.size() == nums.length) {
                break;
            }
            int c = 0;
            for (int i : nums) {
                if (num == i) {
                    c++;
                }
            }
            if (c > 1) {
                if (c > prev) {
                    co = c;
                    prev = c;
                } else if (c == prev) {
                    co = c + co;
                }
            }
        }
        if (co == 0) {
            return nums.length;
        }
        return co;
    }
}
```