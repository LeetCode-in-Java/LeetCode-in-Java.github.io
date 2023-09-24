[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 287\. Find the Duplicate Number

Medium

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return _this repeated number_.

You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

**Example 1:**

**Input:** nums = [1,3,4,2,2]

**Output:** 2 

**Example 2:**

**Input:** nums = [3,1,3,4,2]

**Output:** 3 

**Example 3:**

**Input:** nums = [1,1]

**Output:** 1 

**Example 4:**

**Input:** nums = [1,1,2]

**Output:** 1 

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `nums.length == n + 1`
*   `1 <= nums[i] <= n`
*   All the integers in `nums` appear only **once** except for **precisely one integer** which appears **two or more** times.

**Follow up:**

*   How can we prove that at least one duplicate number must exist in `nums`?
*   Can you solve the problem in linear runtime complexity?

## Solution

```java
public class Solution {
    public int findDuplicate(int[] nums) {
        int[] arr = new int[nums.length + 1];
        for (int num : nums) {
            arr[num] += 1;
            if (arr[num] == 2) {
                return num;
            }
        }
        return 0;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(n), where 'n' is the length of the input array `nums`. This is because the program iterates through the entire array exactly once in a single pass. In the worst case, it might need to iterate through the entire array to find the duplicate.

**Space Complexity (Big O Space):**

The space complexity of the program is O(n), where 'n' is the length of the input array `nums`. The program creates an integer array `arr` of size `nums.length + 1`, which requires additional space. The size of this array is directly proportional to the size of the input array `nums`.

In summary, the provided program has a time complexity of O(n), where 'n' is the length of the input array `nums`, and it has a space complexity of O(n), indicating linear space usage.
