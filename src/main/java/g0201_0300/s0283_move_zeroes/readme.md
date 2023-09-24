[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 283\. Move Zeroes

Easy

Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

**Example 1:**

**Input:** nums = [0,1,0,3,12]

**Output:** [1,3,12,0,0] 

**Example 2:**

**Input:** nums = [0]

**Output:** [0] 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>

**Follow up:** Could you minimize the total number of operations done?

## Solution

```java
public class Solution {
    public void moveZeroes(int[] nums) {
        int firstZero = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                swap(firstZero, i, nums);
                firstZero++;
            }
        }
    }

    private void swap(int index1, int index2, int[] numbers) {
        int val2 = numbers[index2];
        numbers[index2] = numbers[index1];
        numbers[index1] = val2;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(n), where 'n' is the length of the input array `nums`. This is because the program iterates through the entire array exactly once in a single pass. The loop runs 'n' times, and inside the loop, there are constant-time operations such as checking and swapping elements.

**Space Complexity (Big O Space):**

The space complexity of the program is O(1), which indicates constant space usage. Regardless of the size of the input array, the program only uses a constant amount of extra space to store integer variables (`firstZero`, `i`, and `val2`) and does not use any additional data structures whose space requirements depend on the input size. The space used for these variables remains constant.

In summary, the provided program has a time complexity of O(n), where 'n' is the length of the input array `nums`, and it has a space complexity of O(1), indicating constant space usage.
