[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 42\. Trapping Rain Water

Hard

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

**Input:** height = [0,1,0,2,1,0,1,3,2,1,2,1]

**Output:** 6

**Explanation:** The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped. 

**Example 2:**

**Input:** height = [4,2,0,3,2,5]

**Output:** 9 

**Constraints:**

*   `n == height.length`
*   <code>1 <= n <= 2 * 10<sup>4</sup></code>
*   <code>0 <= height[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int trap(int[] height) {
        int l = 0;
        int r = height.length - 1;
        int res = 0;
        int lowerWall = 0;
        while (l < r) {
            int lVal = height[l];
            int rVal = height[r];
            // If left is smaller than right ptr, make the lower wall the bigger of lVal and its
            // current size
            if (lVal < rVal) {
                // If lVal has gone up, move the lowerWall upp
                lowerWall = Math.max(lVal, lowerWall);
                // Add the water level at current point
                // Calculate this by taking the current value and subtracting it from the lower wall
                // size
                // We know that this is the lower wall because we've already determined that lVal <
                // rVal
                res += lowerWall - lVal;
                // Move left ptr along
                l++;
            } else {
                // Do the same thing, except now we know that the lowerWall is the right side.
                lowerWall = Math.max(rVal, lowerWall);
                res += lowerWall - rVal;
                r--;
            }
        }
        return res;
    }
}
```

**Time Complexity (Big O Time):**

1. The program uses two pointers, `l` (left) and `r` (right), that initially start at the leftmost and rightmost elements of the input `height` array. The pointers move towards each other until they meet.

2. In each step of the loop, the program computes the trapped rainwater at the current position based on the lower wall (the smaller of the heights at `height[l]` and `height[r]`).

3. The loop runs until the `l` pointer is less than the `r` pointer, and in each iteration, it either increments `l` or decrements `r`. Since the pointers move towards each other and never move backward, the loop executes at most `n` times, where `n` is the number of elements in the input `height` array.

4. The operations within the loop, including comparisons, arithmetic, and mathematical operations (e.g., `Math.max`), are all constant time operations, so they do not significantly impact the overall time complexity.

5. Therefore, the time complexity of the program is O(n), where `n` is the number of elements in the input array `height`.

**Space Complexity (Big O Space):**

The space complexity of the program is O(1), which means it uses a constant amount of additional space regardless of the size of the input array `height`. The program only uses a few integer variables (`l`, `r`, `res`, `lowerWall`, `lVal`, and `rVal`) and does not use any additional data structures or memory that scales with the input size.

In summary, the time complexity of the provided program is O(n), and the space complexity is O(1), where `n` is the number of elements in the input array `height`.
