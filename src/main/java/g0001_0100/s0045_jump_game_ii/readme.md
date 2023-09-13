[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 45\. Jump Game II

Medium

Given an array of non-negative integers `nums`, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.

**Example 1:**

**Input:** nums = [2,3,1,1,4]

**Output:** 2

**Explanation:** The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index. 

**Example 2:**

**Input:** nums = [2,3,0,1,4]

**Output:** 2 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   `0 <= nums[i] <= 1000`

## Solution

```java
public class Solution {
    public int jump(int[] nums) {
        int length = 0;
        int maxLength = 0;
        int minJump = 0;
        for (int i = 0; i < nums.length - 1; ++i) {
            length--;
            maxLength--;
            maxLength = Math.max(maxLength, nums[i]);
            if (length <= 0) {
                length = maxLength;
                minJump++;
            }
            if (length >= nums.length - i - 1) {
                return minJump;
            }
        }
        return minJump;
    }
}
```

﻿**Time Complexity (Big O Time):**

1. The program uses a single loop that iterates through the elements of the `nums` array from left to right.

2. In each iteration, it performs constant time operations such as arithmetic, comparisons, and calls to `Math.max`. These operations do not depend on the size of the input array.

3. The loop runs until the second-to-last element of the array (i.e., `i < nums.length - 1`), so the number of iterations is at most `n - 1`, where `n` is the number of elements in the input array.

4. Therefore, the overall time complexity of the program is O(n), where `n` is the number of elements in the input array `nums`.

**Space Complexity (Big O Space):**

1. The space complexity of the program is O(1), which means it uses a constant amount of additional space regardless of the size of the input array `nums`. The program only uses a few integer variables (`length`, `maxLength`, `minJump`, and `i`) and does not use any additional data structures or memory that scales with the input size.

In summary, the time complexity of the provided program is O(n), and the space complexity is O(1), where `n` is the number of elements in the input array `nums`.
