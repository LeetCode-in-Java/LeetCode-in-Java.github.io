[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 747\. Largest Number At Least Twice of Others

Easy

You are given an integer array `nums` where the largest integer is **unique**.

Determine whether the largest element in the array is **at least twice** as much as every other number in the array. If it is, return _the **index** of the largest element, or return_ `-1` _otherwise_.

**Example 1:**

**Input:** nums = [3,6,1,0]

**Output:** 1

**Explanation:** 6 is the largest integer. For every other number in the array x, 6 is at least twice as big as x. The index of value 6 is 1, so we return 1.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** -1

**Explanation:** 4 is less than twice the value of 3, so we return -1.

**Example 3:**

**Input:** nums = [1]

**Output:** 0

**Explanation:** 1 is trivially at least twice the value as any other number because there are no other numbers.

**Constraints:**

*   `1 <= nums.length <= 50`
*   `0 <= nums[i] <= 100`
*   The largest element in `nums` is unique.

## Solution

```java
public class Solution {
    public int dominantIndex(int[] arr) {
        if (arr.length == 1) {
            return 0;
        }
        if (arr.length == 0) {
            return -1;
        }

        int maxElement = Integer.MIN_VALUE;
        for (int a : arr) {
            maxElement = Math.max(maxElement, a);
        }
        int index = findNum(maxElement, arr);

        for (int i = 0; i < arr.length; i++) {
            if (i == index) {
                continue;
            }
            if (arr[i] * 2 > maxElement) {
                return -1;
            }
        }

        return index;
    }

    public int findNum(int target, int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;
            }
        }
        return -1;
    }
}
```